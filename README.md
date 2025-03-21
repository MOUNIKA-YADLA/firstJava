
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bulk Account Creation Dashboard</title>
    <style>
        body {
            display: flex;
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background: #f4f4f4;
        }
        .sidebar {
            width: 250px;
            background: #2C3E50;
            color: white;
            padding: 20px;
            height: 100vh;
        }
        .sidebar h2 {
            text-align: center;
        }
        .sidebar ul {
            list-style-type: none;
            padding: 0;
        }
        .sidebar ul li {
            padding: 15px;
        }
        .sidebar ul li a {
            color: white;
            text-decoration: none;
            display: block;
        }
        .sidebar ul li:hover {
            background: #34495E;
        }
        .main-content {
            flex: 1;
            padding: 20px;
        }
        header {
            background: #2980B9;
            color: white;
            padding: 10px;
            text-align: center;
            font-size: 24px;
        }
        .stats {
            display: flex;
            justify-content: space-around;
            margin: 20px 0;
        }
        .stat-box {
            background: #F1C40F;
            padding: 20px;
            border-radius: 5px;
            text-align: center;
            width: 150px;
            box-shadow: 2px 2px 10px rgba(0, 0, 0, 0.2);
        }
        .upload-section {
            text-align: center;
            margin-top: 20px;
            background: white;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 2px 2px 10px rgba(0, 0, 0, 0.2);
        }
        .upload-section input {
            padding: 10px;
            border: 1px solid #ccc;
            margin-right: 10px;
        }
        .upload-section button {
            padding: 10px 20px;
            background: #27ae60;
            color: white;
            border: none;
            cursor: pointer;
            font-size: 16px;
        }
        .upload-section button:hover {
            background: #219150;
        }
        #uploadStatus {
            margin-top: 10px;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <div class="sidebar">
        <h2>Bulk Account Creation</h2>
        <ul>
            <li><a href="#">Dashboard</a></li>
            <li><a href="#">Upload Excel</a></li>
            <li><a href="#">View Accounts</a></li>
            <li><a href="#">Logs & Reports</a></li>
            <li><a href="#">Settings</a></li>
        </ul>
    </div>
    
    <div class="main-content">
        <header>Dashboard</header>
        
        <section class="stats">
            <div class="stat-box">
                <h3>Total Accounts</h3>
                <p>120</p>
            </div>
            <div class="stat-box">
                <h3>Pending Uploads</h3>
                <p>5</p>
            </div>
            <div class="stat-box">
                <h3>Failed Uploads</h3>
                <p>2</p>
            </div>
        </section>

        <section class="upload-section">
            <h2>Upload Excel File</h2>
            <form id="uploadForm" method="post" enctype="multipart/form-data">
                <input type="file" id="fileInput" name="file" required>
                <button type="submit">Upload</button>
            </form>
            <p id="uploadStatus"></p>
        </section>
    </div>

    <script>
        document.getElementById("uploadForm").addEventListener("submit", function(event) {
            event.preventDefault();
            let fileInput = document.getElementById("fileInput").files[0];

            if (!fileInput) {
                alert("Please select a file!");
                return;
            }

            let formData = new FormData();
            formData.append("file", fileInput);

            fetch("/upload-excel", {
                method: "POST",
                body: formData
            })
            .then(response => response.text())
            .then(data => {
                document.getElementById("uploadStatus").innerText = "Upload successful!";
            })
            .catch(error => {
                document.getElementById("uploadStatus").innerText = "Upload failed!";
                console.error("Error:", error);
            });
        });
    </script>
</body>
</html>
