To read a YAML file in Kotlin, convert snake_case fields to camelCase, and save the modified OpenAPI schema into a new file, you can use the following approach:

	1.	Read the YAML file: Use a library like jackson-dataformat-yaml to read and parse the YAML file into a Kotlin data structure (like Map or custom objects).
	2.	Convert snake_case to camelCase: You can traverse the parsed structure and apply a transformation function to convert the keys from snake_case to camelCase.
	3.	Write the modified structure back to a file: After modifying the structure, write the updated data back to a new file in YAML format.

Here is a sample Kotlin code snippet that demonstrates this process:

Dependencies (Gradle)

Add the following dependencies to your build.gradle.kts:

implementation("com.fasterxml.jackson.module:jackson-module-kotlin:2.14.0")
implementation("com.fasterxml.jackson.dataformat:jackson-dataformat-yaml:2.14.0")

Kotlin Code:

import com.fasterxml.jackson.databind.ObjectMapper
import com.fasterxml.jackson.dataformat.yaml.YAMLFactory
import com.fasterxml.jackson.module.kotlin.KotlinModule
import java.io.File

// Helper function to convert snake_case to camelCase
fun snakeToCamel(snakeCase: String): String {
    return snakeCase.split('_').mapIndexed { index, part ->
        if (index == 0) part else part.replaceFirstChar { it.uppercase() }
    }.joinToString("")
}

fun convertKeysToCamelCase(map: Map<String, Any?>): Map<String, Any?> {
    return map.mapKeys { (key, _) -> snakeToCamel(key) }.mapValues { (_, value) ->
        when (value) {
            is Map<*, *> -> convertKeysToCamelCase(value as Map<String, Any?>)
            is List<*> -> value.map {
                if (it is Map<*, *>) {
                    convertKeysToCamelCase(it as Map<String, Any?>)
                } else {
                    it
                }
            }
            else -> value
        }
    }
}

fun main() {
    val objectMapper = ObjectMapper(YAMLFactory()).apply {
        registerModule(KotlinModule())
    }

    // Read the YAML file
    val inputFile = File("input.yaml")
    val schema = objectMapper.readValue(inputFile, Map::class.java) as Map<String, Any?>

    // Convert keys to camelCase
    val camelCaseSchema = convertKeysToCamelCase(schema)

    // Write the modified schema to a new YAML file
    val outputFile = File("output.yaml")
    objectMapper.writeValue(outputFile, camelCaseSchema)

    println("Converted schema saved to output.yaml")
}

Explanation:

	1.	snakeToCamel function: Converts a snake_case string to camelCase.
	2.	convertKeysToCamelCase function: Recursively traverses the map, applying the snakeToCamel function to all keys. It also handles nested maps and lists.
	3.	Main logic:
	•	Reads the input YAML file.
	•	Converts all the snake_case keys to camelCase.
	•	Writes the modified structure to a new YAML file.

This should allow you to process your OpenAPI schema as needed.
