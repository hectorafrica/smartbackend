
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Admin Login - SmartValleyTech Academy</title>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/tailwindcss/2.2.19/tailwind.min.css" rel="stylesheet">
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <style>
        .gradient-bg {
            background: linear-gradient(135deg, #2A0944, #3B185F);
        }
        
        .form-input {
            @apply w-full px-4 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-purple-500 transition duration-200;
        }

        .error-message {
            @apply text-red-500 text-sm mt-1 hidden;
        }

        .loading-spinner {
            border: 3px solid rgba(255, 255, 255, 0.3);
            border-radius: 50%;
            border-top: 3px solid #fff;
            width: 24px;
            height: 24px;
            animation: spin 1s linear infinite;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
    </style>
</head>
<body class="gradient-bg min-h-screen flex items-center justify-center">
    <div class="max-w-md w-full mx-4">
        <!-- Login Card -->
        <div class="bg-white rounded-lg shadow-xl p-8">
            <!-- Logo and Title -->
            <div class="text-center mb-8">
                <h1 class="text-2xl font-bold text-gray-900 mb-2">SmartValleyTech Academy</h1>
                <p class="text-gray-600">Admin Portal</p>
            </div>

            <!-- Login Form -->
            <form id="loginForm" class="space-y-6">
                <!-- Username/Email -->
                <div>
                    <label for="email" class="block text-sm font-medium text-gray-700 mb-1">
                        Email Address
                    </label>
                    <input type="email" id="email" name="email" required
                           class="form-input"
                           placeholder="admin@smartvalleytech.com">
                    <span class="error-message"></span>
                </div>

                <!-- Password -->
                <div>
                    <label for="password" class="block text-sm font-medium text-gray-700 mb-1">
                        Password
                    </label>
                    <div class="relative">
                        <input type="password" id="password" name="password" required
                               class="form-input pr-10"
                               placeholder="Enter your password">
                        <button type="button" id="togglePassword"
                                class="absolute right-3 top-1/2 transform -translate-y-1/2 text-gray-500 hover:text-gray-700">
                            <i class="fas fa-eye"></i>
                        </button>
                    </div>
                    <span class="error-message"></span>
                </div>

                <!-- Remember Me -->
                <div class="flex items-center justify-between">
                    <div class="flex items-center">
                        <input type="checkbox" id="remember" name="remember"
                               class="h-4 w-4 text-purple-600 focus:ring-purple-500 border-gray-300 rounded">
                        <label for="remember" class="ml-2 block text-sm text-gray-700">
                            Remember me
                        </label>
                    </div>
                    <a href="#" class="text-sm text-purple-600 hover:text-purple-500">
                        Forgot password?
                    </a>
                </div>

                <!-- Login Button -->
                <button type="submit"
                        class="w-full flex justify-center py-2 px-4 border border-transparent rounded-md shadow-sm text-sm font-medium text-white bg-purple-600 hover:bg-purple-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-purple-500 transition duration-200">
                    <span class="submit-text">Sign In</span>
                    <div class="loading-spinner ml-3 hidden"></div>
                </button>
            </form>

            <!-- Security Notice -->
            <div class="mt-6 text-center text-sm text-gray-500">
                <i class="fas fa-shield-alt mr-1"></i>
                Secure admin access only
            </div>
        </div>
    </div>

    <script>
        // Toggle password visibility
        const togglePassword = document.getElementById('togglePassword');
        const passwordInput = document.getElementById('password');

        togglePassword.addEventListener('click', () => {
            const type = passwordInput.getAttribute('type') === 'password' ? 'text' : 'password';
            passwordInput.setAttribute('type', type);
            togglePassword.innerHTML = type === 'password' ? 
                '<i class="fas fa-eye"></i>' : 
                '<i class="fas fa-eye-slash"></i>';
        });

        // Form validation and submission
        const loginForm = document.getElementById('loginForm');
        const emailInput = document.getElementById('email');
        const submitButton = loginForm.querySelector('button[type="submit"]');
        const submitText = submitButton.querySelector('.submit-text');
        const spinner = submitButton.querySelector('.loading-spinner');

        function showError(input, message) {
            const errorElement = input.nextElementSibling;
            errorElement.textContent = message;
            errorElement.style.display = 'block';
            input.classList.add('border-red-500');
        }

        function clearError(input) {
            const errorElement = input.nextElementSibling;
            errorElement.style.display = 'none';
            input.classList.remove('border-red-500');
        }

        function validateEmail(email) {
            const re = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
            return re.test(email.toLowerCase());
        }

        loginForm.addEventListener('submit', async (e) => {
            e.preventDefault();
            let isValid = true;

            // Clear previous errors
            clearError(emailInput);
            clearError(passwordInput);

            // Validate email
            if (!emailInput.value.trim()) {
                showError(emailInput, 'Email is required');
                isValid = false;
            } else if (!validateEmail(emailInput.value)) {
                showError(emailInput, 'Please enter a valid email address');
                isValid = false;
            }

            // Validate password
            if (!passwordInput.value.trim()) {
                showError(passwordInput, 'Password is required');
                isValid = false;
            }

            if (isValid) {
                // Show loading state
                submitText.style.opacity = '0';
                spinner.classList.remove('hidden');
                submitButton.disabled = true;

                try {
                    // Simulate API call - Replace with actual authentication
                    await new Promise(resolve => setTimeout(resolve, 1500));

                    // For demo purposes - replace with actual authentication
                    if (emailInput.value === 'admin@smartvalleytech.com' && 
                        passwordInput.value === 'admin123') {
                        // Store authentication token
                        localStorage.setItem('adminToken', 'demo-token');
                        // Redirect to admin dashboard
                        window.location.href = 'admin-dashboard.html';
                    } else {
                        showError(emailInput, 'Invalid email or password');
                        showError(passwordInput, 'Invalid email or password');
                    }
                } catch (error) {
                    console.error('Login error:', error);
                    showError(emailInput, 'Login failed. Please try again.');
                } finally {
                    // Reset button state
                    submitText.style.opacity = '1';
                    spinner.classList.add('hidden');
                    submitButton.disabled = false;
                }
            }
        });

        // Remember me functionality
        const rememberCheckbox = document.getElementById('remember');
        
        // Load remembered email if exists
        const rememberedEmail = localStorage.getItem('rememberedEmail');
        if (rememberedEmail) {
            emailInput.value = rememberedEmail;
            rememberCheckbox.checked = true;
        }

        // Save email if remember me is checked
        rememberCheckbox.addEventListener('change', () => {
            if (rememberCheckbox.checked) {
                localStorage.setItem('rememberedEmail', emailInput.value);
            } else {
                localStorage.removeItem('rememberedEmail');
            }
        });
    </script>
</body>
</html>
