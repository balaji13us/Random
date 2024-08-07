Sure! You can modify the `onSuccess` callback to print out the metadata returned from the Plaid widget. The metadata object contains various details about the transaction, such as account information and institution data.

Here's the updated HTML with jQuery example:

### Full HTML with jQuery Example:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Plaid Integration</title>
    <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
    <script src="https://cdn.plaid.com/link/v2/stable/link-initialize.js"></script>
</head>
<body>
    <h1>Plaid Link Example</h1>
    <form id="plaid-form">
        <button type="submit">Get Public Token</button>
    </form>
    <p id="public-token-label"></p>
    <pre id="metadata-output"></pre>

    <script>
        $(document).ready(function() {
            $('#plaid-form').on('submit', function(e) {
                e.preventDefault();

                const clientId = 'YOUR_PLAID_CLIENT_ID'; // Replace with your Plaid client ID
                const secret = 'YOUR_PLAID_SECRET';     // Replace with your Plaid secret

                $.ajax({
                    url: 'https://sandbox.plaid.com/link/token/create',
                    type: 'POST',
                    contentType: 'application/json',
                    data: JSON.stringify({
                        client_id: clientId,
                        secret: secret,
                        user: {
                            client_user_id: 'unique_user_id'
                        },
                        client_name: 'Your App Name',
                        products: ['auth'],
                        country_codes: ['US'],
                        language: 'en'
                    }),
                    success: function(response) {
                        const linkToken = response.link_token;

                        const handler = Plaid.create({
                            token: linkToken,
                            onSuccess: function(public_token, metadata) {
                                $('#public-token-label').text(`Public Token: ${public_token}`);
                                
                                // Print metadata to a <pre> element for better readability
                                $('#metadata-output').text(JSON.stringify(metadata, null, 2));
                            },
                            onExit: function(err, metadata) {
                                if (err != null) {
                                    console.error(err);
                                }
                            }
                        });

                        handler.open();
                    },
                    error: function(xhr, status, error) {
                        console.error('Error fetching link token:', error);
                    }
                });
            });
        });
    </script>
</body>
</html>
```

### Explanation:
1. **HTML Elements**: Added a `<pre>` element with the id `metadata-output` to display the metadata in a readable format.
2. **AJAX Request**: Remains the same, sending the request to create a link token.
3. **Plaid Link Handler**:
   - **onSuccess Callback**: Displays the public token in the `public-token-label` element and prints the metadata in the `metadata-output` element using `JSON.stringify` with indentation for better readability.

This will allow you to see the detailed metadata returned by the Plaid widget when a user successfully links their account. The metadata can include information such as the accounts linked, the institution name, and more, depending on the Plaid product you're using.
