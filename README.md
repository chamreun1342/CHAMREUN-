<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Share Page Video to Multiple Groups</title>
    <style>
        body {
            font-family: Arial, sans-serif;
        }

        h1 {
            color: #D8A7C2; /* Rosy Pink Color */
        }

        .message-box {
            color: #D8A7C2; /* Rosy Pink Text */
            background-color: #f4f4f4;
            padding: 10px;
            border: 2px solid #D8A7C2;
            border-radius: 5px;
            margin-bottom: 15px;
            font-weight: bold; /* Makes text bold */
        }

        button {
            background-color: #D8A7C2;
            color: white;
            border: none;
            padding: 10px 20px;
            cursor: pointer;
            border-radius: 5px;
        }

        button:hover {
            background-color: #FF69B4; /* Hot Pink on Hover */
        }
    </style>
</head>
<body>

    <h1>Share Video from Page to Multiple Groups</h1>

    <form id="shareForm">
        <label for="videoLink"> Link Video:</label>
        <input type="text" id="videoLink" name="videoLink" required placeholder="https://www.facebook.com/yourpage/videos/12345"><br><br>

        <label for="groupIds">Group ID (comma-separated):</label>
        <input type="text" id="groupIds" name="groupIds" required placeholder="12345,67890,112233"><br><br>

        <label for="message">Message (Optional):</label>
        <textarea id="message" name="message" placeholder="Your message here"></textarea><br><br>

        <button type="button" onclick="shareVideo()">Share Video</button>
    </form>

    <div id="results" class="message-box"></div>

    <script>
        function shareVideo() {
            const videoLink = document.getElementById('videoLink').value;
            const groupIds = document.getElementById('groupIds').value.split(','); // Group IDs separated by commas
            const message = document.getElementById('message').value || ''; // Optional message
            const accessToken = 'YOUR_ACCESS_TOKEN_HERE'; // Replace with your Page Access Token
            const resultsDiv = document.getElementById('results');
            resultsDiv.innerHTML = ''; // Clear previous results

            groupIds.forEach(groupId => {
                const trimmedGroupId = groupId.trim(); // Remove extra spaces
                const url = `https://graph.facebook.com/${trimmedGroupId}/feed`;

                fetch(url, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify({
                        link: videoLink, // Video link to share
                        message: message,
                        access_token: accessToken
                    })
                })
                .then(response => response.json())
                .then(data => {
                    if (data.id) {
                        resultsDiv.innerHTML += `<p><b>Shared successfully to Group ID: ${trimmedGroupId}</b></p>`;
                    } else {
                        resultsDiv.innerHTML += `<p><b>Error sharing to Group ID: ${trimmedGroupId} - ${JSON.stringify(data)}</b></p>`;
                    }
                })
                .catch(error => {
                    resultsDiv.innerHTML += `<p><b>Error sharing to Group ID: ${trimmedGroupId} - ${error}</b></p>`;
                });
            });
        }
    </script>
</body>
</html>
