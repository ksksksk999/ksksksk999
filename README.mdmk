<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Video to MP3 Converter</title>
</head>
<body>
    <h1>Video to MP3 Converter</h1>
    <form id="uploadForm">
        <input type="file" id="fileInput" name="file">
        <button type="submit">Upload</button>
    </form>
    <p id="statusMessage"></p>
    <a id="downloadLink" style="display: none;" href="#" nfs>Download MP3</a>
    <br>
    <audio id="audioPlayer" controls style="display: none;"></audio>

    <script>
        document.getElementById('uploadForm').addEventListener('submit', function(event) {
            event.preventDefault();
            const fileInput = document.getElementById('fileInput');
            const statusMessage = document.getElementById('statusMessage');
            const downloadLink = document.getElementById('downloadLink');
            const audioPlayer = document.getElementById('audioPlayer');

            const formData = new FormData();
            formData.append('file', fileInput.files[0]);

            fetch('/upload', {
                method: 'POST',
                body: formData
            })
            .then(response => response.json())
            .then(data => {
                if (data.error) {
                    statusMessage.textContent = data.error;
                } else {
                    const taskId = data.task_id;
                    statusMessage.textContent = 'Processing...';
                    const checkStatus = setInterval(() => {
                        fetch(`/status/${taskId}`)
                        .then(response => response.json())
                        .then(statusData => {
                            if (statusData.status === 'completed') {
                                clearInterval(checkStatus);
                                statusMessage.textContent = 'Processing completed!';
                                downloadLink.href = `/download/${taskId}`;
                                downloadLink.style.display = 'block';
                                audioPlayer.src = `/download/${taskId}`;
                                audioPlayer.style.display = 'block';
                            } else if (statusData.status.startsWith('error')) {
                                clearInterval(checkStatus);
                                statusMessage.textContent = statusData.status;
                            }
                        });
                    }, 1000);
                }
            });
        });
    </script>
</body>
</html>
