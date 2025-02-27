<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Smart Bin - Intelligent Waste Management</title>
    <link href="https://cdn.jsdelivr.net/npm/flowbite@1.6.5/dist/flowbite.min.css" rel="stylesheet">
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            darkMode: 'media',
            theme: {
                extend: {
                    colors: {
                        primary: '#28a745',
                        primaryDark: '#218838',
                    }
                }
            }
        }
    </script>
    <style>
        :root {
            --primary-color: #28a745;
            --primary-dark: #218838;
            --secondary-color: #e3f2fd;
        }
        
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            text-align: center;
        }
        
        /* Bin Animation Styles */
        #lid {
            width: 120px;
            height: 20px;
            background: var(--primary-color);
            position: absolute;
            top: 0;
            left: 50%;
            transform: translateX(-50%);
            transition: 0.5s;
            border-radius: 10px 10px 0 0;
            z-index: 10;
        }
        
        .bin {
            width: 120px;
            height: 180px;
            background: #ddd;
            border: 2px solid #bbb;
            margin: 20px auto 0;
            border-radius: 0 0 10px 10px;
            position: relative;
            overflow: hidden;
        }
        
        .waste {
            width: 100%;
            height: 0;
            background: var(--primary-color);
            position: absolute;
            bottom: 0;
            transition: 0.5s;
        }
        
        .bin-container {
            position: relative;
            height: 220px;
            width: 130px;
            margin: 40px auto;
        }
        
        .bin::after {
            content: '';
            position: absolute;
            top: 0;
            right: 0;
            width: 20px;
            height: 30px;
            background: rgba(255, 255, 255, 0.2);
            border-radius: 0 0 5px 0;
        }
        
        .sensor {
            position: absolute;
            width: 10px;
            height: 10px;
            background: red;
            border-radius: 50%;
            top: -15px;
            left: 50%;
            transform: translateX(-50%);
            box-shadow: 0 0 5px rgba(255, 0, 0, 0.5);
        }
        
        .sensor.active {
            background: #ff0;
            box-shadow: 0 0 10px rgba(255, 255, 0, 0.7);
        }
        
        @keyframes glow {
            0%, 100% {
                box-shadow: 0 0 5px rgba(255, 0, 0, 0.5);
            }
            50% {
                box-shadow: 0 0 15px rgba(255, 0, 0, 0.8);
            }
        }
        
        .feature {
            transition: transform 0.3s ease, box-shadow 0.3s ease;
        }
        
        .feature:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 20px rgba(0, 0, 0, 0.1);
        }
        
        /* Dark mode styles */
        @media (prefers-color-scheme: dark) {
            body {
                background-color: #181818;
                color: #fff;
            }
            
            .feature {
                background-color: #2a2a2a;
                color: #f8f9fa;
            }
            
            .bin {
                background: #555;
                border-color: #666;
            }
            
            input, textarea {
                background-color: #333;
                color: #fff;
                border-color: #555;
            }
        }
    </style>
</head>
<body class="bg-gray-50 dark:bg-gray-900">
    <header class="bg-primary dark:bg-primary text-white py-6 px-4 shadow-md">
        <div class="container mx-auto">
            <h1 class="text-2xl md:text-3xl font-bold">EcoBin Solutions - Smart Bin</h1>
            <p class="text-lg">Intelligent Waste Management</p>
        </div>
    </header>

    <main class="container mx-auto px-4 py-8">
        <section class="bg-white dark:bg-gray-800 rounded-lg shadow-lg p-6 mb-8">
            <h2 class="text-2xl font-bold text-primary dark:text-green-400 mb-4">Revolutionizing Waste Disposal</h2>
            <p class="text-gray-700 dark:text-gray-300 mb-6">Our Smart Bin helps you dispose of waste in a more efficient, hygienic, and eco-friendly way.</p>
            
            <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-8">
                <div class="feature bg-blue-50 dark:bg-gray-700 p-4 rounded-lg">
                    <h3 class="text-xl font-semibold text-primary dark:text-green-400 mb-2">👐 Touchless Lid</h3>
                    <p class="text-gray-700 dark:text-gray-300">Automatically opens when motion is detected, ensuring a hygienic experience!</p>
                </div>
                <div class="feature bg-blue-50 dark:bg-gray-700 p-4 rounded-lg">
                    <h3 class="text-xl font-semibold text-primary dark:text-green-400 mb-2">📊 Smart Waste Level</h3>
                    <p class="text-gray-700 dark:text-gray-300">Monitors waste levels and alerts when the bin is full!</p>
                </div>
                <div class="feature bg-blue-50 dark:bg-gray-700 p-4 rounded-lg">
                    <h3 class="text-xl font-semibold text-primary dark:text-green-400 mb-2">🔋 Energy Efficient</h3>
                    <p class="text-gray-700 dark:text-gray-300">Operates on low power and is rechargeable!</p>
                </div>
                <div class="feature bg-blue-50 dark:bg-gray-700 p-4 rounded-lg">
                    <h3 class="text-xl font-semibold text-primary dark:text-green-400 mb-2">🌍 Eco-friendly Materials</h3>
                    <p class="text-gray-700 dark:text-gray-300">Our product is made using 100% sustainable materials!</p>
                </div>
            </div>

            <h3 class="text-xl font-bold text-primary dark:text-green-400 mb-4">Smart Bin Simulation</h3>
            <div class="bin-container">
                <div id="lid"></div>
                <div class="sensor" id="sensor"></div>
                <div class="bin">
                    <div class="waste" id="wasteLevel"></div>
                </div>
            </div>
            
            <div class="flex flex-wrap justify-center gap-2 mt-4">
                <button onclick="openLid()" class="bg-primary hover:bg-primaryDark text-white font-medium py-2 px-4 rounded-lg transition duration-300 flex items-center">
                    <svg class="w-5 h-5 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 15l7-7 7 7"></path>
                    </svg>
                    Open Lid
                </button>
                <button onclick="closeLid()" class="bg-primary hover:bg-primaryDark text-white font-medium py-2 px-4 rounded-lg transition duration-300 flex items-center">
                    <svg class="w-5 h-5 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7"></path>
                    </svg>
                    Close Lid
                </button>
                <button onclick="addWaste()" class="bg-primary hover:bg-primaryDark text-white font-medium py-2 px-4 rounded-lg transition duration-300 flex items-center">
                    <svg class="w-5 h-5 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 4v16m8-8H4"></path>
                    </svg>
                    Add Waste
                </button>
                <button onclick="emptyBin()" class="bg-primary hover:bg-primaryDark text-white font-medium py-2 px-4 rounded-lg transition duration-300 flex items-center">
                    <svg class="w-5 h-5 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16"></path>
                    </svg>
                    Empty Bin
                </button>
            </div>
        </section>

        <section class="bg-white dark:bg-gray-800 rounded-lg shadow-lg p-6">
            <h2 class="text-2xl font-bold text-primary dark:text-green-400 mb-4">Contact Us</h2>
            <p class="text-gray-700 dark:text-gray-300 mb-6">Have questions? Get in touch with us!</p>
            
            <form onsubmit="submitForm(event)" class="max-w-lg mx-auto">
                <div class="mb-4">
                    <label for="name" class="block text-gray-700 dark:text-gray-300 mb-2 text-left">Your Name</label>
                    <input type="text" id="name" placeholder="John Doe" required 
                           class="w-full px-4 py-2 text-base border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-primary focus:border-primary dark:bg-gray-700 dark:text-white">
                </div>
                
                <div class="mb-4">
                    <label for="email" class="block text-gray-700 dark:text-gray-300 mb-2 text-left">Your Email</label>
                    <input type="email" id="email" placeholder="john@example.com" required 
                           class="w-full px-4 py-2 text-base border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-primary focus:border-primary dark:bg-gray-700 dark:text-white">
                </div>
                
                <div class="mb-4">
                    <label for="message" class="block text-gray-700 dark:text-gray-300 mb-2 text-left">Your Message</label>
                    <textarea id="message" rows="4" placeholder="Tell us what you need..." required 
                              class="w-full px-4 py-2 text-base border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-primary focus:border-primary dark:bg-gray-700 dark:text-white"></textarea>
                </div>
                
                <button type="submit" class="w-full bg-primary hover:bg-primaryDark text-white font-medium py-3 px-6 rounded-lg transition duration-300">
                    Send Message
                </button>
            </form>
            
            <div id="form-message" class="hidden mt-4 p-4 bg-green-100 dark:bg-green-900 text-green-700 dark:text-green-300 rounded-lg">
                Thank you! We will get back to you soon.
            </div>
        </section>
    </main>

    <footer class="bg-gray-100 dark:bg-gray-800 mt-12 py-8 px-4">
        <div class="container mx-auto text-center">
            <p class="text-gray-700 dark:text-gray-300">© 2023 EcoBin Solutions. All rights reserved.</p>
            <div class="flex justify-center space-x-4 mt-4">
                <a href="#" class="text-primary hover:text-primaryDark dark:text-green-400 dark:hover:text-green-300">
                    <svg class="w-6 h-6" fill="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                        <path d="M22.675 0h-21.35c-.732 0-1.325.593-1.325 1.325v21.351c0 .731.593 1.324 1.325 1.324h11.495v-9.294h-3.128v-3.622h3.128v-2.671c0-3.1 1.893-4.788 4.659-4.788 1.325 0 2.463.099 2.795.143v3.24l-1.918.001c-1.504 0-1.795.715-1.795 1.763v2.313h3.587l-.467 3.622h-3.12v9.293h6.116c.73 0 1.323-.593 1.323-1.325v-21.35c0-.732-.593-1.325-1.325-1.325z"/>
                    </svg>
                </a>
                <a href="#" class="text-primary hover:text-primaryDark dark:text-green-400 dark:hover:text-green-300">
                    <svg class="w-6 h-6" fill="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                        <path d="M23.953 4.57a10 10 0 01-2.825.775 4.958 4.958 0 002.163-2.723 10.054 10.054 0 01-3.127 1.184 4.92 4.92 0 00-8.384 4.482C7.69 8.095 4.067 6.13 1.64 3.162a4.822 4.822 0 00-.666 2.475c0 1.71.87 3.213 2.188 4.096a4.904 4.904 0 01-2.228-.616v.06a4.923 4.923 0 003.946 4.83 4.996 4.996 0 01-2.212.085 4.936 4.936 0 004.604 3.417 9.867 9.867 0 01-6.102 2.105c-.39 0-.779-.023-1.17-.067a13.995 13.995 0 007.557 2.209c9.053 0 13.998-7.496 13.998-13.985 0-.21 0-.42-.015-.63A9.935 9.935 0 0024 4.59z"/>
                    </svg>
                </a>
                <a href="#" class="text-primary hover:text-primaryDark dark:text-green-400 dark:hover:text-green-300">
                    <svg class="w-6 h-6" fill="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                        <path d="M12 0C5.373 0 0 5.373 0 12s5.373 12 12 12 12-5.373 12-12S18.627 0 12 0zm4.441 16.892c-2.102.144-6.784.144-8.883 0C5.282 16.736 5.017 15.622 5 12c.017-3.629.285-4.736 2.558-4.892 2.099-.144 6.782-.144 8.883 0C18.718 7.264 18.982 8.378 19 12c-.018 3.629-.285 4.736-2.559 4.892zM10 9.658l4.917 2.338L10 14.342V9.658z"/>
                    </svg>
                </a>
                <a href="#" class="text-primary hover:text-primaryDark dark:text-green-400 dark:hover:text-green-300">
                    <svg class="w-6 h-6" fill="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                        <path d="M12 0C5.373 0 0 5.372 0 12c0 5.084 3.163 9.426 7.627 11.174-.105-.949-.2-2.405.042-3.441.218-.937 1.407-5.965 1.407-5.965s-.359-.719-.359-1.782c0-1.668.967-2.914 2.171-2.914 1.023 0 1.518.769 1.518 1.69 0 1.029-.655 2.568-.994 3.995-.283 1.194.599 2.169 1.777 2.169 2.133 0 3.772-2.249 3.772-5.495 0-2.873-2.064-4.882-5.012-4.882-3.414 0-5.418 2.561-5.418 5.207 0 1.031.397 2.138.893 2.738.098.119.112.224.083.345l-.333 1.36c-.053.22-.174.267-.402.161-1.499-.698-2.436-2.889-2.436-4.649 0-3.785 2.75-7.262 7.929-7.262 4.163 0 7.398 2.967 7.398 6.931 0 4.136-2.607 7.464-6.227 7.464-1.216 0-2.359-.631-2.75-1.378l-.748 2.853c-.271 1.043-1.002 2.35-1.492 3.146C9.57 23.812 10.763 24 12 24c6.627 0 12-5.373 12-12 0-6.628-5.373-12-12-12z"/>
                    </svg>
                </a>
            </div>
        </div>
    </footer>

    <script>
        const sensor = document.getElementById("sensor");
        
        function openLid() {
            document.getElementById("lid").style.top = "-30px";
            sensor.classList.add("active");
            
            // Automatically close lid after 3 seconds
            setTimeout(() => {
                closeLid();
            }, 3000);
        }

        function closeLid() {
            document.getElementById("lid").style.top = "0px";
            sensor.classList.remove("active");
        }

        function addWaste() {
            let waste = document.getElementById("wasteLevel");
            let currentHeight = parseInt(waste.style.height) || 0;
            
            // First open the lid
            openLid();
            
            // Then add waste after a short delay
            setTimeout(() => {
                if (currentHeight < 100) {
                    waste.style.height = (currentHeight + 20) + "%";
                }
                
                // Show alert when bin is getting full
                if (currentHeight >= 80) {
                    // Create a notification div instead of using alert
                    const notification = document.createElement('div');
                    notification.className = 'fixed top-4 right-4 bg-red-500 text-white px-4 py-2 rounded-lg shadow-lg z-50';
                    notification.innerHTML = '<div class="flex items-center"><svg class="w-5 h-5 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 9v2m0 4h.01m-6.938 4h13.856c1.54 0 2.502-1.667 1.732-3L13.732 4c-.77-1.333-2.694-1.333-3.464 0L3.34 16c-.77 1.333.192 3 1.732 3z"></path></svg>Smart Bin Alert: The bin is almost full!</div>';
                    document.body.appendChild(notification);
                    
                    // Remove the notification after 3 seconds
                    setTimeout(() => {
                        notification.remove();
                    }, 3000);
                }
            }, 500);
        }

        function emptyBin() {
            openLid();
            
            setTimeout(() => {
                document.getElementById("wasteLevel").style.height = "0%";
                
                // Show empty notification
                const notification = document.createElement('div');
                notification.className = 'fixed top-4 right-4 bg-green-500 text-white px-4 py-2 rounded-lg shadow-lg z-50';
                notification.innerHTML = '<div class="flex items-center"><svg class="w-5 h-5 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"></path></svg>Bin emptied successfully!</div>';
                document.body.appendChild(notification);
                
                // Remove the notification after 3 seconds
                setTimeout(() => {
                    notification.remove();
                }, 3000);
            }, 500);
        }

        function submitForm(event) {
            event.preventDefault();
            const formMessage = document.getElementById("form-message");
            formMessage.classList.remove("hidden");
            
            // Clear form fields
            document.getElementById("name").value = "";
            document.getElementById("email").value = "";
            document.getElementById("message").value = "";
            
            // Hide message after 3 seconds
            setTimeout(() => {
                formMessage.classList.add("hidden");
            }, 3000);
        }
        
        // Add automatic sensor animation
        setInterval(() => {
            sensor.style.animation = "glow 2s infinite";
        }, 5000);
    </script>
</body>
</html>
