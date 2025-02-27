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
            --secondary-color: black;
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

        /* Brochure styles */
        .brochure-page {
            background: white;
            border: 1px solid #ddd;
            border-radius: 8px;
            box-shadow: 0 5px 15px rgba(0,0,0,0.1);
            overflow: hidden;
            position: relative;
            height: 100%;
            display: flex;
            flex-direction: column;
        }
        
        .brochure-content {
            flex: 1;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
        }
        
        .animation-container {
            height: 160px;
            display: flex;
            align-items: center;
            justify-content: center;
            position: relative;
        }
        
        .solar-animation {
            position: relative;
            height: 50px;
            width: 100%;
            max-width: 180px;
        }
        
        .solar-panel {
            width: 100%;
            height: 30px;
            background: linear-gradient(45deg, #1a3c6e, #2d6fb8);
            position: relative;
            border-radius: 4px;
            overflow: hidden;
        }
        
        .sun {
            position: absolute;
            width: 20px;
            height: 20px;
            background: #ffcc00;
            border-radius: 50%;
            top: -10px;
            left: 20px;
            box-shadow: 0 0 20px #ffcc00;
            transition: left 4s ease-in-out;
        }
        
        .energy-beam {
            position: absolute;
            width: 0;
            height: 0;
            border-left: 10px solid transparent;
            border-right: 10px solid transparent;
            border-top: 40px solid rgba(255, 204, 0, 0.3);
            top: 5px;
            left: 25px;
            transform-origin: top;
            animation: beamPulse 3s infinite;
            transition: left 4s ease-in-out;
        }
        
        @keyframes beamPulse {
            0%, 100% { opacity: 0.3; }
            50% { opacity: 0.7; }
        }
        
        .brochure-bin {
            width: 80px;
            height: 120px;
            background: #f5f5f5;
            border: 2px solid #ddd;
            border-radius: 0 0 8px 8px;
            position: relative;
            overflow: hidden;
            margin: 0 auto;
        }
        
        .brochure-lid {
            width: 80px;
            height: 15px;
            background: var(--primary-color);
            position: absolute;
            top: 0;
            left: 50%;
            transform: translateX(-50%);
            transition: all 0.5s ease;
            border-radius: 8px 8px 0 0;
            z-index: 2;
        }
        
        .brochure-waste {
            width: 100%;
            height: 10%;
            background: var(--primary-color);
            position: absolute;
            bottom: 0;
            transition: height 3s ease;
        }
        
        .brochure-sensor {
            width: 12px;
            height: 12px;
            background: red;
            border-radius: 50%;
            position: absolute;
            top: -8px;
            left: 50%;
            transform: translateX(-50%);
            z-index: 3;
            box-shadow: 0 0 5px rgba(255, 0, 0, 0.5);
            transition: all 0.5s ease;
        }
        
        .level-indicator {
            position: absolute;
            right: 8px;
            height: 70%;
            width: 6px;
            background: rgba(255, 255, 255, 0.3);
            top: 15%;
            border-radius: 3px;
            overflow: hidden;
        }
        
        .level-marker {
            width: 10px;
            height: 10px;
            border-radius: 50%;
            position: absolute;
            left: -2px;
            transform: translateX(-50%);
        }
        
        .battery-container {
            width: 80px;
            height: 30px;
            background: rgba(240, 240, 240, 0.8);
            border-radius: 15px;
            margin: 10px auto 0;
            display: flex;
            align-items: center;
            padding: 0 5px;
            border: 1px solid #ddd;
            position: relative;
        }
        
        .battery-indicator {
            width: 20px;
            height: 20px;
            background: white;
            border-radius: 50%;
            transition: transform 1.5s ease-in-out;
            box-shadow: 0 0 5px rgba(0, 0, 0, 0.2);
            position: absolute;
            left: 5px;
        }
        
        .feature-list {
            text-align: left;
            margin-top: 15px;
        }
        
        .feature-list li {
            margin-bottom: 5px;
            font-size: 14px;
        }

        /* Dark mode styles */
        @media (prefers-color-scheme: dark) {
            body {
                background-color: white;
                color: black;
            }
            
            .feature {
                background-color: white;
                color: black;
            }
            
            .bin {
                background: white;
                border-color: green;
            }
            
            input, textarea {
                background-color: white;
                color: black;
                border-color: green;
            }

            .dark\:bg-gray-900,
            .dark\:bg-gray-800,
            .dark\:bg-gray-700,
            .dark\:bg-green-900 {
                background-color: white !important;
            }

            .dark\:text-gray-300,
            .dark\:text-white,
            .dark\:text-green-300,
            .dark\:text-green-400 {
                color: black !important;
            }
            
            .dark\:border-gray-600 {
                border-color: #28a745 !important;
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

        <!-- Brochure section -->
        <section class="bg-white dark:bg-gray-800 rounded-lg shadow-lg p-6 mb-8">
            <h2 class="text-2xl font-bold text-primary dark:text-green-400 mb-4">Smart Bin Brochure</h2>
            <p class="text-gray-700 dark:text-gray-300 mb-6">Learn about the revolutionary features of our EcoBin Smart Waste System.</p>
            
            <div class="grid grid-cols-1 lg:grid-cols-3 gap-6 mb-8">
                <!-- Brochure Page 1: Auto Open Lid -->
                <div class="brochure-page">
                    <div class="pb-4 border-b border-gray-200">
                        <h3 class="text-xl font-semibold text-primary mb-2">Touchless Open System</h3>
                    </div>
                    <div class="brochure-content p-4">
                        <div class="animation-container">
                            <div class="relative">
                                <div class="brochure-lid" id="brochureLid"></div>
                                <div class="brochure-sensor" id="brochureSensor"></div>
                                <div class="brochure-bin">
                                    <div class="flex items-center justify-center h-full">
                                        <svg class="w-10 h-10 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M7 11.5V14m0-2.5v-6a1.5 1.5 0 113 0m-3 6a1.5 1.5 0 00-3 0v2a7.5 7.5 0 0015 0v-5a1.5 1.5 0 00-3 0m-6-3V11m0-5.5v-1a1.5 1.5 0 013 0v1m0 0V11m0-5.5a1.5 1.5 0 013 0v3m0 0V11"></path>
                                        </svg>
                                    </div>
                                </div>
                            </div>
                        </div>
                        <div class="feature-list">
                            <ul class="list-disc ml-6 text-gray-700 dark:text-gray-300">
                                <li>Motion sensors detect movement up to 20cm away</li>
                                <li>Lid opens automatically within 0.5 seconds</li>
                                <li>Stays open until motion stops</li>
                                <li>Hygienic & convenient - perfect for kitchens and bathrooms</li>
                            </ul>
                        </div>
                    </div>
                </div>
                
                <!-- Brochure Page 2: Waste Detection -->
                <div class="brochure-page">
                    <div class="pb-4 border-b border-gray-200">
                        <h3 class="text-xl font-semibold text-primary mb-2">Smart Waste Detection</h3>
                    </div>
                    <div class="brochure-content p-4">
                        <div class="animation-container">
                            <div class="brochure-bin">
                                <div class="brochure-waste" id="brochureWaste"></div>
                                <div class="level-indicator">
                                    <div class="level-marker" style="top: 20%; background-color: #ffd700;"></div>
                                    <div class="level-marker" style="top: 50%; background-color: #ffa500;"></div>
                                    <div class="level-marker" style="top: 80%; background-color: #ff0000;"></div>
                                </div>
                            </div>
                        </div>
                        <div class="feature-list">
                            <ul class="list-disc ml-6 text-gray-700 dark:text-gray-300">
                                <li>Ultrasonic sensors monitor fill level in real-time</li>
                                <li>Color-coded indicators show waste level status</li>
                                <li>Smart notifications when bin reaches 80% capacity</li>
                                <li>Prevents overflow and maintains hygiene</li>
                            </ul>
                        </div>
                    </div>
                </div>
                
                <!-- Brochure Page 3: Solar Charging -->
                <div class="brochure-page">
                    <div class="pb-4 border-b border-gray-200">
                        <h3 class="text-xl font-semibold text-primary mb-2">Solar Powered System</h3>
                    </div>
                    <div class="brochure-content p-4">
                        <div class="animation-container">
                            <div class="flex flex-col items-center w-full">
                                <div class="solar-animation mb-4">
                                    <div class="solar-panel">
                                        <div class="sun" id="brochureSun"></div>
                                        <div class="energy-beam" id="energyBeam"></div>
                                    </div>
                                </div>
                                <div class="brochure-bin" style="height: 80px;">
                                    <div class="battery-container">
                                        <div class="battery-indicator" id="batteryIndicator"></div>
                                        <div class="text-xs font-semibold absolute right-3 text-green-700">ON</div>
                                    </div>
                                </div>
                            </div>
                        </div>
                        <div class="feature-list">
                            <ul class="list-disc ml-6 text-gray-700 dark:text-gray-300">
                                <li>High-efficiency solar panel charges throughout the day</li>
                                <li>Integrated 2000mAh rechargeable battery</li>
                                <li>Up to 3 months operation on a single charge</li>
                                <li>Eco-friendly design reduces carbon footprint</li>
                            </ul>
                        </div>
                    </div>
                </div>
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
            <p class="text-gray-700 dark:text-gray-300">© 2025 EcoBin Solutions. All rights reserved.</p>
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

        // Initialize brochure elements
        document.addEventListener('DOMContentLoaded', function() {
            const brochureLid = document.getElementById("brochureLid");
            const brochureSensor = document.getElementById("brochureSensor");
            const brochureWaste = document.getElementById("brochureWaste");
            const brochureSun = document.getElementById("brochureSun");
            const batteryIndicator = document.getElementById("batteryIndicator");
            const energyBeam = document.getElementById("energyBeam");

            // Set initial states
            brochureWaste.style.height = "10%";
            brochureSun.style.left = "20px";
            batteryIndicator.style.transform = "translateX(0)";
            
            // Setup animations
            let lidOpen = false;
            let wasteIncreasing = true;
            let sunMovingRight = true;
            let batteryOn = false;
            
            // Lid and sensor animation
            setInterval(() => {
                if (lidOpen) {
                    brochureLid.style.transform = "translate(-50%, -100%)";
                    brochureSensor.style.backgroundColor = "#ffff00";
                    brochureSensor.style.boxShadow = "0 0 10px rgba(255, 255, 0, 0.7)";
                    setTimeout(() => {
                        lidOpen = false;
                    }, 2000);
                } else {
                    brochureLid.style.transform = "translate(-50%, 0)";
                    brochureSensor.style.backgroundColor = "#ff0000";
                    brochureSensor.style.boxShadow = "0 0 5px rgba(255, 0, 0, 0.5)";
                    setTimeout(() => {
                        lidOpen = true;
                    }, 2000);
                }
            }, 4000);
            
            // Waste level animation
            setInterval(() => {
                const currentHeight = parseInt(brochureWaste.style.height) || 10;
                if (wasteIncreasing) {
                    if (currentHeight < 75) {
                        brochureWaste.style.height = (currentHeight + 15) + "%";
                    } else {
                        wasteIncreasing = false;
                    }
                } else {
                    if (currentHeight > 10) {
                        brochureWaste.style.height = (currentHeight - 15) + "%";
                    } else {
                        wasteIncreasing = true;
                    }
                }
            }, 3000);
            
            // Sun and energy beam animation
            setInterval(() => {
                const currentLeft = parseInt(brochureSun.style.left) || 20;
                if (sunMovingRight) {
                    if (currentLeft < 140) {
                        brochureSun.style.left = (currentLeft + 20) + "px";
                        energyBeam.style.left = (currentLeft + 25) + "px";
                    } else {
                        sunMovingRight = false;
                    }
                } else {
                    if (currentLeft > 20) {
                        brochureSun.style.left = (currentLeft - 20) + "px";
                        energyBeam.style.left = (currentLeft - 15) + "px";
                    } else {
                        sunMovingRight = true;
                    }
                }
            }, 1500);
            
            // Battery indicator animation
            setInterval(() => {
                if (batteryOn) {
                    batteryIndicator.style.transform = "translateX(0)";
                    batteryOn = false;
                } else {
                    batteryIndicator.style.transform = "translateX(40px)";
                    batteryOn = true;
                }
            }, 2000);
        });
    </script>
</body>
</html>
