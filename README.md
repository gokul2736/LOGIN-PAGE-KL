# LOGIN-PAGE-KL
Next.js components file 

utf-8
meta data inappropriate*

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>KaptureLab - Login</title>
    
    <!-- Tailwind CSS for styling -->
    <script src="https://cdn.tailwindcss.com"></script>
    
    <!-- Google Fonts -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@300;400;500;600;700&display=swap" rel="stylesheet">

    <!-- Custom styles for animation and layout -->
    <style>
        /* Use Montserrat as the default font */
        body {
            font-family: 'Montserrat', sans-serif;
        }

        /* Base styles for the sliding forms and overlay */
        .form-container {
            position: absolute;
            top: 0;
            height: 100%;
            transition: all 0.6s ease-in-out;
        }

        .sign-in-container {
            left: 0;
            width: 50%;
            z-index: 2;
        }

        .sign-up-container {
            left: 0;
            width: 50%;
            opacity: 0;
            z-index: 1;
        }

        .toggle-container {
            position: absolute;
            top: 0;
            left: 50%;
            width: 50%;
            height: 100%;
            overflow: hidden;
            transition: all 0.6s ease-in-out;
            border-radius: 150px 0 0 100px;
            z-index: 1000;
        }

        .toggle {
            background-color: #512da8;
            background: linear-gradient(to right, #5c6bc0, #512da8);
            color: #fff;
            position: relative;
            left: -100%;
            height: 100%;
            width: 200%;
            transform: translateX(0);
            transition: all 0.6s ease-in-out;
        }

        .toggle-panel {
            position: absolute;
            width: 50%;
            height: 100%;
            display: flex;
            align-items: center;
            justify-content: center;
            flex-direction: column;
            padding: 0 30px;
            text-align: center;
            top: 0;
            transform: translateX(0);
            transition: all 0.6s ease-in-out;
        }

        .toggle-left {
            transform: translateX(-200%);
        }

        .toggle-right {
            right: 0;
            transform: translateX(0);
        }

        /* Active state transformations (when user clicks 'Continue as Guest') */
        .container.active .sign-in-container {
            transform: translateX(100%);
        }

        .container.active .sign-up-container {
            transform: translateX(100%);
            opacity: 1;
            z-index: 5;
            animation: move 0.6s;
        }

        @keyframes move {
            0%, 49.99% {
                opacity: 0;
                z-index: 1;
            }
            50%, 100% {
                opacity: 1;
                z-index: 5;
            }
        }

        .container.active .toggle-container {
            transform: translateX(-100%);
            border-radius: 0 150px 100px 0;
        }

        .container.active .toggle {
            transform: translateX(50%);
        }

        .container.active .toggle-left {
            transform: translateX(0);
        }

        .container.active .toggle-right {
            transform: translateX(200%);
        }
    </style>
</head>
<body class="bg-gradient-to-r from-gray-200 to-blue-200 flex justify-center items-center h-screen">
    <div id="root"></div>

    <!-- React and Babel for in-browser JSX transformation -->
    <script src="https://unpkg.com/react@17/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom@17/umd/react-dom.development.js"></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>

    <script type="text/babel">
        // --- SVG Icons as React Components ---
        const GithubIcon = ({ className }) => (
            <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round" className={className}>
                <path d="M9 19c-5 1.5-5-2.5-7-3m14 6v-3.87a3.37 3.37 0 0 0-.94-2.61c3.14-.35 6.44-1.54 6.44-7A5.44 5.44 0 0 0 20 4.77 5.07 5.07 0 0 0 19.91 1S18.73.65 16 2.48a13.38 13.38 0 0 0-7 0C6.27.65 5.09 1 5.09 1A5.07 5.07 0 0 0 5 4.77a5.44 5.44 0 0 0-1.5 3.78c0 5.42 3.3 6.61 6.44 7A3.37 3.37 0 0 0 9 18.13V22"></path>
            </svg>
        );

        const GoogleIcon = ({ className }) => (
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 48 48" width="48px" height="48px" className={className}>
                <path fill="#fbc02d" d="M43.611,20.083H42V20H24v8h11.303c-1.649,4.657-6.08,8-11.303,8c-6.627,0-12-5.373-12-12 s5.373-12,12-12c3.059,0,5.842,1.154,7.961,3.039l5.657-5.657C34.046,6.053,29.268,4,24,4C12.955,4,4,12.955,4,24s8.955,20,20,20 s20-8.955,20-20C44,22.659,43.862,21.35,43.611,20.083z"></path>
                <path fill="#e53935" d="M6.306,14.691l6.571,4.819C14.655,15.108,18.961,12,24,12c3.059,0,5.842,1.154,7.961,3.039 l5.657-5.657C34.046,6.053,29.268,4,24,4C16.318,4,9.656,8.337,6.306,14.691z"></path>
                <path fill="#4caf50" d="M24,44c5.166,0,9.86-1.977,13.409-5.192l-6.19-5.238C29.211,35.091,26.715,36,24,36 c-5.222,0-9.619-3.317-11.283-7.946l-6.522,5.025C9.505,39.556,16.227,44,24,44z"></path>
                <path fill="#1565c0" d="M43.611,20.083H42V20H24v8h11.303c-0.792,2.237-2.231,4.166-4.087,5.574l6.19,5.238 C42.022,35.283,44,30.038,44,24C44,22.659,43.862,21.35,43.611,20.083z"></path>
            </svg>
        );

        // --- The Main Authentication Form Component ---
        const AuthForm = () => {
            const [isActive, setIsActive] = React.useState(false);

            const activeClasses = isActive ? 'active' : '';

            return (
                <div className={`container relative overflow-hidden rounded-3xl bg-white shadow-lg w-[768px] max-w-full min-h-[480px] ${activeClasses}`}>
                    {/* Guest Sign Up Form */}
                    <div className="form-container sign-up-container">
                        <form className="bg-white flex items-center justify-center flex-col px-10 h-full text-center">
                            <h1 className="text-3xl md:text-4xl font-bold">Sign in as a Guest</h1>
                            <span className="text-xs my-4">Enter your details to continue</span>
                            <input 
                                type="text" 
                                placeholder="Name" 
                                className="bg-gray-100 border-none my-2 p-3 rounded-lg w-full outline-none focus:ring-2 focus:ring-purple-400 focus:border-purple-600 transition-all duration-300" 
                            />
                            <input 
                                type="text" 
                                placeholder="WhatsApp Number" 
                                className="bg-gray-100 border-none my-2 p-3 rounded-lg w-full outline-none focus:ring-2 focus:ring-purple-400 focus:border-purple-600 transition-all duration-300" 
                            />
                            <button type="button" className="mt-4 bg-[#512da8] hover:bg-[#5c6bc0] text-white text-xs font-bold uppercase tracking-wider py-3 px-11 rounded-lg transition-colors">
                                Continue as Guest
                            </button>
                        </form>
                    </div>

                    {/* Social Sign In Form */}
                    <div className="form-container sign-in-container">
                        <form className="bg-white flex items-center justify-center flex-col px-10 h-full text-center">
                            <h1 className="text-3xl md:text-4xl font-bold">Sign In</h1>
                            <span className="text-xs my-4">Sign in using a provider below</span>
                            <div className="grid grid-cols-1 gap-3 w-full max-w-xs">
                                <button type="button" className="group bg-white border border-gray-300 hover:bg-gradient-to-r hover:from-[#5c6bc0] hover:to-[#512da8] hover:text-white text-gray-800 font-semibold py-2 px-4 rounded-lg flex items-center justify-center transition-all duration-300">
                                    <GithubIcon className="mr-2 h-5 w-5 group-hover:stroke-white transition-colors" />
                                    Sign in with GitHub
                                </button>
                                <button type="button" className="group bg-white border border-gray-300 hover:bg-gradient-to-r hover:from-[#5c6bc0] hover:to-[#512da8] hover:text-white text-gray-800 font-semibold py-2 px-4 rounded-lg flex items-center justify-center transition-all duration-300">
                                    <GoogleIcon className="mr-2 h-5 w-5" />
                                    Sign in with Google
                                </button>
                            </div>
                        </form>
                    </div>

                    {/* Toggle Overlay */}
                    <div className="toggle-container">
                        <div className="toggle">
                            {/* Left Overlay Panel (shows when guest form is active) */}
                            <div className="toggle-panel toggle-left">
                                <h1 className="text-2xl font-bold">Welcome Back!</h1>
                                <p className="text-sm my-4 px-4">To access your account, please use the sign-in options.</p>
                                <button
                                    className="bg-transparent border border-white hover:bg-white/20 text-white text-xs font-bold uppercase tracking-wider py-3 px-11 rounded-lg transition-colors"
                                    onClick={() => setIsActive(false)}
                                >
                                    Sign In
                                </button>
                            </div>
                            
                            {/* Right Overlay Panel (shows on initial load) */}
                            <div className="toggle-panel toggle-right">
                                <h1 className="text-2xl font-bold">Continue as Guest!</h1>
                                <p className="text-sm my-4 px-4">No account needed. Just provide basic details to generate your record.</p>
                                <button
                                    className="bg-transparent border border-white hover:bg-white/20 text-white text-xs font-bold uppercase tracking-wider py-3 px-11 rounded-lg transition-colors"
                                    onClick={() => setIsActive(true)}
                                >
                                    Continue as Guest
                                </button>
                            </div>
                        </div>
                    </div>
                </div>
            );
        };

        // --- Main App Component ---
        const App = () => {
            return (
                <main className="p-4">
                    <AuthForm />
                </main>
            );
        };

        // Render the App to the DOM
        ReactDOM.render(<App />, document.getElementById('root'));
    </script>
</body>
</html>

```
