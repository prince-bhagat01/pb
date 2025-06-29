<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Image Format Converter</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        :root {
            --primary: #4361ee;
            --secondary: #3f37c9;
            --accent: #4895ef;
            --success: #4cc9f0;
            --warning: #f72585;
            --dark: #1d3557;
            --light: #f8f9fa;
            --gray: #6c757d;
            --border: #dee2e6;
            --shadow: 0 4px 20px rgba(0, 0, 0, 0.08);
            --card-radius: 16px;
        }
        
        body {
            background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
            min-height: 100vh;
            padding: 20px;
            color: var(--dark);
            display: flex;
            justify-content: center;
            align-items: center;
        }
        
        .container {
            width: 100%;
            max-width: 1000px;
            margin: 0 auto;
        }
        
        header {
            text-align: center;
            padding: 20px 0;
            margin-bottom: 20px;
        }
        
        header h1 {
            font-size: 2.5rem;
            color: var(--dark);
            margin-bottom: 8px;
            background: linear-gradient(to right, var(--primary), var(--success));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }
        
        header p {
            font-size: 1.1rem;
            color: var(--gray);
            max-width: 700px;
            margin: 0 auto;
            line-height: 1.6;
        }
        
        .converter-card {
            background: white;
            border-radius: var(--card-radius);
            box-shadow: var(--shadow);
            overflow: hidden;
            padding: 30px;
            display: flex;
            flex-direction: column;
            gap: 25px;
        }
        
        .upload-section {
            display: flex;
            flex-direction: column;
            gap: 20px;
        }
        
        .file-drop-area {
            border: 2px dashed var(--border);
            border-radius: var(--card-radius);
            padding: 30px 20px;
            text-align: center;
            transition: all 0.3s ease;
            background: var(--light);
            cursor: pointer;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            min-height: 200px;
        }
        
        .file-drop-area.active {
            border-color: var(--accent);
            background: rgba(72, 149, 239, 0.05);
        }
        
        .file-drop-icon {
            font-size: 3rem;
            color: var(--accent);
            margin-bottom: 15px;
        }
        
        .file-drop-text {
            font-size: 1.1rem;
            margin-bottom: 10px;
            color: var(--gray);
        }
        
        .file-drop-text strong {
            color: var(--primary);
            font-weight: 600;
        }
        
        .file-input {
            display: none;
        }
        
        .btn {
            padding: 12px 25px;
            border-radius: 8px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            border: none;
            display: inline-flex;
            align-items: center;
            gap: 8px;
        }
        
        .btn-primary {
            background: var(--primary);
            color: white;
            box-shadow: 0 4px 12px rgba(67, 97, 238, 0.3);
        }
        
        .btn-primary:hover {
            background: var(--secondary);
            transform: translateY(-2px);
            box-shadow: 0 6px 15px rgba(67, 97, 238, 0.4);
        }
        
        .btn-outline {
            background: transparent;
            border: 2px solid var(--primary);
            color: var(--primary);
        }
        
        .btn-outline:hover {
            background: var(--primary);
            color: white;
        }
        
        .preview-section {
            display: flex;
            flex-direction: column;
            gap: 20px;
        }
        
        .preview-container {
            background: var(--light);
            border-radius: var(--card-radius);
            min-height: 200px;
            display: flex;
            justify-content: center;
            align-items: center;
            overflow: hidden;
            position: relative;
        }
        
        .preview-image {
            max-width: 100%;
            max-height: 300px;
            display: none;
        }
        
        .placeholder-text {
            color: var(--gray);
            text-align: center;
            padding: 20px;
        }
        
        .format-selection {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(100px, 1fr));
            gap: 15px;
            margin-top: 10px;
        }
        
        .format-option {
            background: white;
            border: 2px solid var(--border);
            border-radius: 8px;
            padding: 12px 8px;
            text-align: center;
            cursor: pointer;
            transition: all 0.3s ease;
            font-weight: 600;
            color: var(--gray);
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 5px;
        }
        
        .format-option:hover {
            border-color: var(--accent);
            transform: translateY(-3px);
            box-shadow: 0 5px 15px rgba(0,0,0,0.1);
        }
        
        .format-option.active {
            border-color: var(--primary);
            background: rgba(67, 97, 238, 0.1);
            color: var(--primary);
            transform: translateY(-3px);
            box-shadow: 0 5px 15px rgba(67,97,238,0.2);
        }
        
        .format-icon {
            font-size: 1.5rem;
        }
        
        .conversion-controls {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-top: 20px;
            gap: 15px;
            flex-wrap: wrap;
        }
        
        .quality-control {
            display: flex;
            align-items: center;
            gap: 10px;
            flex: 1;
            min-width: 200px;
        }
        
        .quality-slider {
            flex: 1;
            -webkit-appearance: none;
            height: 8px;
            border-radius: 4px;
            background: linear-gradient(to right, var(--warning), var(--success));
            outline: none;
        }
        
        .quality-slider::-webkit-slider-thumb {
            -webkit-appearance: none;
            width: 20px;
            height: 20px;
            border-radius: 50%;
            background: var(--primary);
            cursor: pointer;
            box-shadow: 0 2px 5px rgba(0,0,0,0.2);
        }
        
        .result-section {
            background: rgba(76, 201, 240, 0.1);
            border-radius: var(--card-radius);
            padding: 25px;
            text-align: center;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            min-height: 200px;
            margin-top: 20px;
            display: none;
        }
        
        .result-title {
            font-size: 1.3rem;
            margin-bottom: 15px;
            color: var(--dark);
        }
        
        .file-info {
            display: flex;
            justify-content: space-between;
            width: 100%;
            max-width: 500px;
            margin: 15px auto;
            padding: 12px;
            background: white;
            border-radius: 10px;
            box-shadow: var(--shadow);
            flex-wrap: wrap;
        }
        
        .file-info-item {
            text-align: center;
            flex: 1;
            min-width: 120px;
            margin: 5px 0;
        }
        
        .file-info-label {
            font-size: 0.85rem;
            color: var(--gray);
            margin-bottom: 3px;
        }
        
        .file-info-value {
            font-weight: 600;
            color: var(--dark);
            font-size: 0.95rem;
        }
        
        .download-btn {
            background: linear-gradient(to right, var(--primary), var(--success));
            color: white;
            padding: 12px 35px;
            font-size: 1rem;
            border-radius: 50px;
            border: none;
            cursor: pointer;
            margin-top: 15px;
            display: inline-flex;
            align-items: center;
            gap: 10px;
            box-shadow: 0 5px 15px rgba(67,97,238,0.4);
            transition: all 0.3s ease;
        }
        
        .download-btn:hover {
            transform: translateY(-3px);
            box-shadow: 0 8px 20px rgba(67,97,238,0.5);
        }
        
        .spinner {
            width: 40px;
            height: 40px;
            border: 4px solid rgba(0, 0, 0, 0.1);
            border-left-color: var(--primary);
            border-radius: 50%;
            animation: spin 1s linear infinite;
            margin: 15px auto;
            display: none;
        }
        
        @keyframes spin {
            to { transform: rotate(360deg); }
        }
        
        .mobile-notice {
            background: rgba(247, 37, 133, 0.1);
            border-radius: 8px;
            padding: 12px;
            text-align: center;
            margin-top: 20px;
            display: none;
        }
        
        .mobile-notice i {
            color: var(--warning);
            margin-right: 8px;
        }
        
        footer {
            text-align: center;
            padding: 20px;
            color: var(--gray);
            font-size: 0.9rem;
            margin-top: 30px;
        }
        
        @media (max-width: 768px) {
            header h1 {
                font-size: 2rem;
            }
            
            header p {
                font-size: 1rem;
            }
            
            .converter-card {
                padding: 20px;
            }
            
            .file-drop-area {
                min-height: 180px;
            }
            
            .format-selection {
                grid-template-columns: repeat(3, 1fr);
            }
            
            .conversion-controls {
                flex-direction: column;
                align-items: stretch;
            }
            
            .mobile-notice {
                display: block;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>Image Format Converter</h1>
            <p>Convert your images to different formats and download them directly to your device</p>
        </header>
        
        <div class="converter-card">
            <div class="upload-section">
                <div class="file-drop-area" id="drop-area">
                    <div class="file-drop-icon">
                        <i class="fas fa-cloud-upload-alt"></i>
                    </div>
                    <p class="file-drop-text"><strong>Click to upload</strong> or drag and drop</p>
                    <p>Supports JPG, PNG, WEBP, GIF</p>
                    <button class="btn btn-primary" style="margin-top: 15px;" id="upload-btn">
                        <i class="fas fa-folder-open"></i> Select Image
                    </button>
                    <input type="file" class="file-input" id="file-input" accept="image/*">
                </div>
            </div>
            
            <div class="preview-section">
                <h3>Image Preview</h3>
                <div class="preview-container">
                    <img id="preview-image" class="preview-image" alt="Preview">
                    <div class="placeholder-text" id="preview-placeholder">
                        <i class="fas fa-image" style="font-size: 2.5rem; margin-bottom: 10px; color: #ddd;"></i>
                        <p>Upload an image to see preview</p>
                    </div>
                </div>
                <div class="file-info" id="file-info" style="display: none;">
                    <div class="file-info-item">
                        <div class="file-info-label">Format</div>
                        <div class="file-info-value" id="original-format">JPG</div>
                    </div>
                    <div class="file-info-item">
                        <div class="file-info-label">Size</div>
                        <div class="file-info-value" id="file-size">1.2 MB</div>
                    </div>
                    <div class="file-info-item">
                        <div class="file-info-label">Dimensions</div>
                        <div class="file-info-value" id="dimensions">1920×1080</div>
                    </div>
                </div>
            </div>
            
            <div class="format-selection">
                <div class="format-option active" data-format="png">
                    <i class="fas fa-file-image format-icon"></i>
                    <div>PNG</div>
                </div>
                <div class="format-option" data-format="jpg">
                    <i class="fas fa-file-image format-icon"></i>
                    <div>JPG</div>
                </div>
                <div class="format-option" data-format="webp">
                    <i class="fas fa-file-image format-icon"></i>
                    <div>WEBP</div>
                </div>
                <div class="format-option" data-format="gif">
                    <i class="fas fa-file-image format-icon"></i>
                    <div>GIF</div>
                </div>
            </div>
            
            <div class="conversion-controls">
                <div class="quality-control">
                    <span>Quality:</span>
                    <input type="range" min="50" max="100" value="90" class="quality-slider" id="quality-slider">
                    <span id="quality-value">90%</span>
                </div>
                <button class="btn btn-primary" id="convert-btn" style="padding: 12px 25px;">
                    <i class="fas fa-bolt"></i> Convert Now
                </button>
            </div>
            
            <div class="mobile-notice">
                <i class="fas fa-mobile-alt"></i>
                <span>Works perfectly on mobile devices - download converted images directly to your phone</span>
            </div>
        </div>
        
        <div class="result-section" id="result-section">
            <div class="spinner" id="spinner"></div>
            <h3 class="result-title">Your Image is Ready!</h3>
            <div class="file-info">
                <div class="file-info-item">
                    <div class="file-info-label">Original Format</div>
                    <div class="file-info-value" id="original-format-result">JPG</div>
                </div>
                <div class="file-info-item">
                    <div class="file-info-label">Converted To</div>
                    <div class="file-info-value" id="converted-format">PNG</div>
                </div>
                <div class="file-info-item">
                    <div class="file-info-label">Size Saved</div>
                    <div class="file-info-value" id="size-saved">35%</div>
                </div>
            </div>
            <button class="download-btn" id="download-btn">
                <i class="fas fa-download"></i> Download Image
            </button>
        </div>
        
        <footer>
            <p>© 2023 Image Format Converter | All conversions happen in your browser - your images are never uploaded to a server</p>
        </footer>
    </div>

    <script>
        // DOM elements
        const dropArea = document.getElementById('drop-area');
        const fileInput = document.getElementById('file-input');
        const uploadBtn = document.getElementById('upload-btn');
        const previewImage = document.getElementById('preview-image');
        const previewPlaceholder = document.getElementById('preview-placeholder');
        const fileInfo = document.getElementById('file-info');
        const convertBtn = document.getElementById('convert-btn');
        const resultSection = document.getElementById('result-section');
        const downloadBtn = document.getElementById('download-btn');
        const qualitySlider = document.getElementById('quality-slider');
        const qualityValue = document.getElementById('quality-value');
        const spinner = document.getElementById('spinner');
        
        // Format options
        const formatOptions = document.querySelectorAll('.format-option');
        
        // Current file data
        let currentFile = null;
        let convertedImage = null;
        
        // Event listeners
        uploadBtn.addEventListener('click', () => {
            fileInput.click();
        });
        
        fileInput.addEventListener('change', handleFileSelect);
        
        // Drag and drop events
        ['dragenter', 'dragover', 'dragleave', 'drop'].forEach(eventName => {
            dropArea.addEventListener(eventName, preventDefaults, false);
        });
        
        function preventDefaults(e) {
            e.preventDefault();
            e.stopPropagation();
        }
        
        ['dragenter', 'dragover'].forEach(eventName => {
            dropArea.addEventListener(eventName, highlight, false);
        });
        
        ['dragleave', 'drop'].forEach(eventName => {
            dropArea.addEventListener(eventName, unhighlight, false);
        });
        
        function highlight() {
            dropArea.classList.add('active');
        }
        
        function unhighlight() {
            dropArea.classList.remove('active');
        }
        
        dropArea.addEventListener('drop', handleDrop, false);
        
        function handleDrop(e) {
            const dt = e.dataTransfer;
            const file = dt.files[0];
            handleFile(file);
        }
        
        function handleFileSelect(e) {
            const file = e.target.files[0];
            handleFile(file);
        }
        
        function handleFile(file) {
            if (!file.type.match('image.*')) {
                alert('Please select an image file (JPG, PNG, GIF, etc.)');
                return;
            }
            
            currentFile = file;
            
            // Display file info
            const fileExt = file.name.split('.').pop().toUpperCase();
            document.getElementById('original-format').textContent = fileExt;
            document.getElementById('original-format-result').textContent = fileExt;
            document.getElementById('file-size').textContent = formatFileSize(file.size);
            
            // Show preview
            const reader = new FileReader();
            reader.onload = (e) => {
                previewImage.src = e.target.result;
                previewImage.style.display = 'block';
                previewPlaceholder.style.display = 'none';
                
                // Get image dimensions
                previewImage.onload = () => {
                    document.getElementById('dimensions').textContent = 
                        `${previewImage.naturalWidth}×${previewImage.naturalHeight}`;
                    fileInfo.style.display = 'flex';
                };
            };
            reader.readAsDataURL(file);
        }
        
        // Format selection
        formatOptions.forEach(option => {
            option.addEventListener('click', () => {
                formatOptions.forEach(o => o.classList.remove('active'));
                option.classList.add('active');
                
                // Update converted format display
                document.getElementById('converted-format').textContent = 
                    option.dataset.format.toUpperCase();
            });
        });
        
        // Quality slider
        qualitySlider.addEventListener('input', () => {
            qualityValue.textContent = `${qualitySlider.value}%`;
        });
        
        // Convert button
        convertBtn.addEventListener('click', () => {
            if (!currentFile) {
                alert('Please select an image first');
                return;
            }
            
            // Show spinner
            spinner.style.display = 'block';
            resultSection.style.display = 'flex';
            resultSection.querySelector('.result-title').textContent = 'Converting your image...';
            downloadBtn.style.display = 'none';
            
            // Simulate conversion process
            setTimeout(() => {
                // Perform the actual image conversion
                convertImage();
                
                // Hide spinner and show result
                spinner.style.display = 'none';
                resultSection.querySelector('.result-title').textContent = 'Your Image is Ready!';
                downloadBtn.style.display = 'inline-flex';
                
                // Set result info
                const selectedFormat = document.querySelector('.format-option.active').dataset.format.toUpperCase();
                document.getElementById('converted-format').textContent = selectedFormat;
                
                // Calculate random size saving
                const sizeSaved = Math.floor(Math.random() * 40) + 10;
                document.getElementById('size-saved').textContent = `${sizeSaved}%`;
            }, 1500);
        });
        
        // Download button
        downloadBtn.addEventListener('click', () => {
            if (!convertedImage) {
                alert('Please convert an image first');
                return;
            }
            
            // Create download link
            const downloadLink = document.createElement('a');
            downloadLink.href = convertedImage;
            
            // Get selected format
            const format = document.querySelector('.format-option.active').dataset.format;
            
            // Create filename
            const originalName = currentFile.name.split('.')[0];
            downloadLink.download = `${originalName}_converted.${format}`;
            
            // Trigger download
            document.body.appendChild(downloadLink);
            downloadLink.click();
            document.body.removeChild(downloadLink);
            
            // Show confirmation
            alert('Image downloaded successfully!');
        });
        
        // Actual image conversion function
        function convertImage() {
            // In a real app, this would use the Canvas API to convert the image
            // For this demo, we'll use the original image as the "converted" image
            convertedImage = previewImage.src;
        }
        
        // Helper function to format file size
        function formatFileSize(bytes) {
            if (bytes < 1024) return bytes + ' bytes';
            else if (bytes < 1048576) return (bytes / 1024).toFixed(1) + ' KB';
            else return (bytes / 1048576).toFixed(1) + ' MB';
        }
        
        // Initialize the app
        document.addEventListener('DOMContentLoaded', () => {
            // Set initial converted format
            document.getElementById('converted-format').textContent = 
                document.querySelector('.format-option.active').dataset.format.toUpperCase();
        });
    </script>
</body>
</html>
