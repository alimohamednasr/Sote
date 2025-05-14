<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>ملفي الشخصي - [اسمك هنا أو عنوان أكثر تحديداً]</title> <link rel="icon" href="/favicon.ico" type="image/x-icon">
  <script src="https://cdn.tailwindcss.com"></script>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/animate.css/4.1.1/animate.min.css"/>
  <script src="https://unpkg.com/scrollreveal"></script>

  <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>

  <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;700&display=swap" rel="stylesheet">
  <style>
    body {
      font-family: 'Cairo', sans-serif;
      background: linear-gradient(135deg, #0f0c29, #302b63, #24243e); /* Default Dark Background */
      color: #ffffff; /* Default Dark Text Color */
      padding-bottom: 80px; /* Add padding to the bottom to prevent fixed button from hiding content */
      transition: background-color 0.5s ease, color 0.5s ease; /* Smooth transition for theme change */
    }
    .glow-text {
      text-shadow: 0 0 10px #00f0ff, 0 0 20px #00f0ff;
    }
    /* rgb-border and rgb-border-animated as originally defined */
    .rgb-border {
      border: 2px solid;
      border-image: linear-gradient(to right, red, green, blue) 1;
    }
    .rgb-border-animated {
      border: 2px solid;
      animation: rgb-border-animation 5s linear infinite;
    }
    @keyframes rgb-border-animation {
      0% { border-image: linear-gradient(to right, red, green, blue) 1; }
      25% { border-image: linear-gradient(to right, green, blue, red) 1; }
      50% { border-image: linear-gradient(to right, blue, red, green) 1; }
      75% { border-image: linear-gradient(to right, red, green, blue) 1; }
      100% { border-image: linear-gradient(to right, green, blue, red) 1; }
    }
    /* rgb-text-animated as originally defined */
    .rgb-text-animated {
      animation: rgb-text-animation 5s linear infinite;
      display: inline-block;
    }
    @keyframes rgb-text-animation {
      0% { color: red; }
      33% { color: green; }
      66% { color: blue; }
      100% { color: red; }
    }

    /* --- Loader Styles --- */
    #loader-overlay {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: linear-gradient(135deg, #0f0c29, #302b63, #24243e); /* Match body background */
      display: flex;
      justify-content: center;
      align-items: center;
      z-index: 9999; /* Make sure it's on top of everything */
      opacity: 1;
      visibility: visible;
      transition: opacity 0.7s ease-out, visibility 0.7s ease-out; /* Smooth transition for hiding */
    }

    #loader-overlay.hidden {
      opacity: 0;
      visibility: hidden;
      pointer-events: none; /* Prevent interaction with hidden overlay */
    }

    /* Loader content styles */
    #loader-overlay p {
        color: #ffffff; /* White text */
        font-size: 2rem; /* Large text */
        font-weight: bold;
    }

    /* --- Fixed WhatsApp Button Styles --- */
    #fixed-whatsapp-button {
      position: fixed;
      bottom: 20px; /* Distance from the bottom */
      right: 20px; /* Distance from the right (for RTL) */
      z-index: 1000; /* Make sure it's above most content */
      /* Initial state handled by ScrollReveal/JS opacity/visibility */
      opacity: 0; /* Initially hidden */
      visibility: hidden;
      transition: opacity 0.5s ease, transform 0.5s ease; /* Add transition */
    }

    /* Ensure the fixed button is responsive */
    @media (max-width: 640px) {
        #fixed-whatsapp-button {
            bottom: 15px;
            right: 15px;
            padding: 0.75rem 1rem; /* Adjust padding */
            font-size: 0.875rem; /* Adjust font size */
        }
         body {
            padding-bottom: 60px; /* Adjust padding for smaller screens */
        }
        #fixed-whatsapp-button svg {
             height: 1.25rem; /* Adjust icon size */
             width: 1.25rem;
        }
    }

    /* --- Theme Toggle Button Styles --- */
    #theme-toggle {
        position: absolute; /* Position it relative to the header */
        top: 1rem;
        left: 1rem; /* Left side for RTL */
        z-index: 50; /* Above header text, below loader */
        cursor: pointer; /* Indicate it's clickable */
    }
    #theme-toggle svg {
        display: block; /* Default: show sun icon (for dark mode) */
    }
    #theme-toggle svg.moon-icon {
        display: none; /* Default: hide moon icon */
    }

    /* --- Light Mode Styles --- */
    body.light-mode {
      background: linear-gradient(135deg, #e0e0e0, #f5f5f5, #cccccc); /* Lighter background */
      color: #333333; /* Darker text */
    }

    body.light-mode .glow-text {
        text-shadow: 0 0 5px rgba(0, 0, 0, 0.3); /* Adjust glow for dark text */
        color: #333333; /* Ensure text color is dark by default in light mode */
    }
    /* Override specific glow text colors in light mode */
    body.light-mode header h1.glow-text { color: #333333; text-shadow: 0 0 5px rgba(0,0,0,0.3); }
    body.light-mode #about h2.glow-text { color: #0891b2; text-shadow: 0 0 5px rgba(8,145,178,0.3); } /* Cyan */
    body.light-mode #skills h2.glow-text { color: #a78bfa; text-shadow: 0 0 5px rgba(167,139,250,0.3); } /* Purple */
    body.light-mode #contact h2.glow-text { color: #16a34a; text-shadow: 0 0 5px rgba(22,163,74,0.3); } /* Green */


    body.light-mode .bg-black\/30 {
        background: rgba(255, 255, 255, 0.7); /* Lighter card background */
        box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1); /* Darker shadow for contrast */
    }

    body.light-mode .text-gray-300 {
        color: #555555; /* Darker gray text */
    }

    /* Adjust specific Tailwind text colors for light mode */
    /* Add transitions for smoother color changes */
    body.light-mode .text-cyan-300 { color: #06b6d4; transition: color 0.5s ease; }
    body.light-mode .text-cyan-400 { color: #0891b2; transition: color 0.5s ease; }
    body.light-mode .text-purple-300 { color: #a78bfa; transition: color 0.5s ease; }
    body.light-mode .text-yellow-300 { color: #d97706; transition: color 0.5s ease; }
    body.light-mode .text-sky-400 { color: #0284c7; transition: color 0.5s ease; }
    body.light-mode .text-green-400 { color: #16a34a; transition: color 0.5s ease; }
    body.light-mode .text-blue-400 { color: #2563eb; transition: color 0.5s ease; }
    body.light-mode .text-pink-400 { color: #e879f9; transition: color 0.5s ease; }
    body.light-mode .text-emerald-400 { color: #059669; transition: color 0.5s ease; }
    body.light-mode .text-red-500 { color: #dc2626; transition: color 0.5s ease; }
    body.light-mode .text-yellow-500 { color: #ca8a04; transition: color 0.5s ease; }
    body.light-mode .text-indigo-400 { color: #6366f1; transition: color 0.5s ease; }
    body.light-mode .text-orange-500 { color: #f97316; transition: color 0.5s ease; }
    body.light-mode .text-teal-400 { color: #14b8a6; transition: color 0.5s ease; }
    body.light-mode .text-lime-400 { color: #84cc16; transition: color 0.5s ease; }
    body.light-mode .text-gray-400 { color: #666666; transition: color 0.5s ease; }
    body.light-mode .text-gray-500 { color: #777777; transition: color 0.5s ease; }

    /* Adjust specific Tailwind background colors for light mode if needed */
    /* body.light-mode .bg-blue-600 { background-color: #3b82f6; } */ /* Example */


    body.light-mode .rgb-border-animated {
      /* Keep the RGB border animation - it might look different on a light background */
    }
    body.light-mode .rgb-text-animated {
         animation: none; /* Disable RGB text animation in light mode for readability */
         color: #333333; /* Set a dark color */
         transition: color 0.5s ease;
    }


    /* Handle skill card background specifically in light mode */
    body.light-mode #skills .grid > div {
        background: rgba(255, 255, 255, 0.9); /* Slightly less transparent white */
        box-shadow: 0 2px 4px rgba(0, 0, 0, 0.08);
         transition: background-color 0.5s ease, box-shadow 0.5s ease;
    }

     /* Handle progress bar background in light mode */
     body.light-mode #skills .w-full.bg-gray-700 {
         background-color: #dddddd; /* Lighter background for the bar track */
     }


    /* Toggle icon display based on body class */
    #theme-toggle svg.sun-icon { /* Sun icon */
        display: block;
    }

    #theme-toggle svg.moon-icon { /* Moon icon */
        display: none;
    }

    body.light-mode #theme-toggle svg.sun-icon {
        display: none;
    }

    body.light-mode #theme-toggle svg.moon-icon {
        display: block;
    }

    /* --- Dialog/Modal Styles (Improved) --- */
    #copyright-dialog {
        border: none;
        padding: 0; /* Remove default padding */
        border-radius: 0.5rem; /* rounded-lg */
        box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05); /* shadow-xl */
        backdrop-filter: blur(8px); /* backdrop-blur-md */
        background: rgba(0, 0, 0, 0.85); /* Darker semi-transparent background */
        color: #ffffff; /* text-white */
        text-align: center;
        max-width: 95%; /* Make it slightly wider on small screens */
        width: 450px; /* Specific width for larger screens */
        /* Removed transition here, will use animation for entry/exit */
        position: fixed; /* Ensure it's positioned correctly */
        top: 50%;
        left: 50%; /* Centering technique */
        transform: translate(-50%, -50%) scale(0.9); /* Start slightly smaller and centered, initial state for animation */
        opacity: 0; /* Start hidden */
        pointer-events: none; /* Don't block clicks when hidden */
        /* Use transition for smooth close animation if not using exit animation */
        transition: transform 0.3s ease-in, opacity 0.3s ease-in;
    }

    /* Style when the dialog is open */
    #copyright-dialog[open] {
        opacity: 1; /* Fade in */
        transform: translate(-50%, -50%) scale(1); /* Scale to normal size */
        pointer-events: auto; /* Enable clicks when open */
        /* Apply entry animation when opened */
        animation: dialog-entry 0.5s cubic-bezier(0.25, 0.8, 0.25, 1.2); /* Bounce-like entry */
    }

     /* Keyframes for the entry animation */
    @keyframes dialog-entry {
        from {
            opacity: 0;
            transform: translate(-50%, -50%) scale(0.8); /* Start smaller and slightly off center visually */
        }
        to {
            opacity: 1;
            transform: translate(-50%, -50%) scale(1); /* End at normal size, perfectly centered */
        }
    }

     /* Style for backdrop */
    #copyright-dialog::backdrop {
        background: rgba(0, 0, 0, 0.7); /* Darken the background */
        backdrop-filter: blur(5px); /* Blur the background */
        /* Match dialog animation or use a simple fade */
        opacity: 0; /* Start hidden */
        transition: opacity 0.3s ease-out; /* Smooth fade for backdrop */
    }

    /* Style for backdrop when dialog is open */
    #copyright-dialog[open]::backdrop {
         opacity: 1; /* Fade in backdrop when dialog is open */
    }


     #copyright-dialog .p-6 {
         padding: 1.5rem; /* p-6 */
     }

     #copyright-dialog .text-xl { font-size: 1.25rem; } /* text-xl */
     #copyright-dialog .mb-4 { margin-bottom: 1rem; } /* mb-4 */
     #copyright-dialog .font-bold { font-weight: 700; } /* font-bold */
     #copyright-dialog .mb-6 { margin-bottom: 1.5rem; } /* mb-6 */
     #copyright-dialog .leading-loose { line-height: 2; } /* leading-loose */
     /* text-gray-300 and text-yellow-400 handled by Tailwind classes, adjust in light mode */


    /* --- Light Mode Styles for Dialog --- */
    body.light-mode #copyright-dialog {
        background: rgba(255, 255, 255, 0.9); /* Lighter semi-transparent background */
        color: #333333; /* Dark text */
        box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.15), 0 4px 6px -2px rgba(0, 0, 0, 0.08); /* Darker shadow */
         /* Keep backdrop-filter */
         /* Keep RGB border if applied */
    }

    body.light-mode #copyright-dialog::backdrop {
         background: rgba(255, 255, 255, 0.7); /* Lighter backdrop */
         backdrop-filter: blur(5px); /* Keep blur */
    }

    /* Ensure specific text colors are handled in light mode */
    body.light-mode #copyright-dialog .text-gray-300 { color: #555555; }
    body.light-mode #copyright-mode #copyright-dialog .text-yellow-400 { color: #d97706; } /* Darker yellow for contrast */


    /* Added style for Vue mount point visibility handled by ScrollReveal */
     /* Initially hide the content inside #skills-app for SR reveal */
     #skills-app {
         opacity: 0; /* Use opacity for smooth transition */
         transition: opacity 0.8s ease-out; /* Smooth transition */
     }
     /* When ScrollReveal reveals the parent section, make content visible */
     section#skills.is-revealed #skills-app {
          opacity: 1;
     }

    /* Ensure skill card is visually correct with opacity */
    #skills-app > div > div { /* Target skill card divs */
        transition: opacity 0.5s ease, transform 0.5s ease, background-color 0.5s ease, box-shadow 0.5s ease; /* Add opacity to transitions */
    }

  </style>
</head>
<body class="min-h-screen overflow-x-hidden">

  <div id="loader-overlay">
    <p class="animate__animated animate__pulse animate__infinite">
      جاري التحميل...
    </p>
  </div>

  <header class="text-center py-16 relative">
    <button id="theme-toggle" class="p-2 rounded-full bg-gray-700 text-white hover:bg-gray-600 transition" aria-label="Toggle light and dark theme"> <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 sun-icon" fill="none" viewBox="0 0 24 24" stroke="currentColor">
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 3v1m0 16v1m9-9h-1M4 12H3m15.364 6.364l-.707-.707M6.343 6.343l-.707-.707m12.728 0l-.707.707M6.343 17.657l-.707.707M16 12a4 4 0 11-8 0 4 4 0 018 0z" />
      </svg>
      <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 moon-icon" fill="none" viewBox="0 0 24 24" stroke="currentColor">
        <path stroke-linecap="round" stroke-linejoin="round" d="M20.354 15.354A9 9 0 018.646 3.646 9.003 9.003 0 0012 21a9.003 9.003 0 008.354-5.646z" />
      </svg>
    </button>
    <h1 class="text-5xl font-bold glow-text animate__animated animate__heartBeat animate__infinite">مطور واجهات أمامية</h1>
    <p class="mt-4 text-xl text-gray-300">مواقع تفاعلية وسريعة بتصميم عصري</p>
  </header>

  <section id="about" class="max-w-4xl mx-auto px-6 py-10 bg-black/30 rounded-xl backdrop-blur-md shadow-xl rgb-border-animated" aria-labelledby="about-heading"> <h2 id="about-heading" class="text-3xl font-bold text-cyan-400 mb-4 glow-text">من أنا؟</h2> <p class="text-gray-300 leading-loose">
      أنا مطور واجهات أمامية باستخدام <span class="text-cyan-300 font-semibold">HTML, CSS, Tailwind, JavaScript, React</span> لإنشاء مواقع حديثة وسريعة الاستجابة. أسعى دائمًا لتعلم تقنيات جديدة وتوسيع نطاق مهاراتي لتشمل تطوير تطبيقات متكاملة وعالية الأداء.
    </p>
  </section>

  <section id="skills" class="max-w-6xl mx-auto px-6 py-10 mt-10 bg-black/30 rounded-xl backdrop-blur-md shadow-xl rgb-border-animated" aria-labelledby="skills-heading"> <h2 id="skills-heading" class="text-3xl font-bold text-purple-300 mb-8 glow-text text-center"><span class="rgb-text-animated">مهاراتي التقنية</span></h2> <div id="skills-app">
        <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-8 text-center">
            <skill-card v-for="skill in skills" :key="skill.name" :skill="skill"></skill-card>
        </div>
    </div>
    </section>

  <section id="contact" class="max-w-4xl mx-auto px-6 py-16 mt-10 bg-black/30 rounded-xl backdrop-blur-md shadow-xl rgb-border-animated text-center" aria-labelledby="contact-heading"> <h2 id="contact-heading" class="text-3xl font-bold text-green-400 mb-4 glow-text animate__animated animate__bounceIn">تواصل معي</h2> <p class="text-gray-400 mb-8">يمكنك التواصل معي عبر إحدى الطرق التالية لمناقشة مشروعك أو الفرص المتاحة:</p>
    <a href="mailto:your.email@example.com" class="inline-block bg-blue-600 hover:bg-blue-700 text-white font-bold py-3 px-8 rounded-full shadow-xl transition-transform transform hover:scale-105">
        <svg xmlns="http://www.w3.org/2000/svg" class="inline-block h-6 w-6 mr-2" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
          <path stroke-linecap="round" stroke-linejoin="round" d="M3 8l7.89 5.26a2 2 0 002.22 0L21 8m-18 9h18a2 2 0 002-2V7a2 2 0 00-2-2H3a2 2 0 00-2 2v10a2 2 0 002 2z" />
        </svg>
        البريد الإلكتروني
    </a>
  </section>

  <footer class="text-center py-8 mt-8">
    <p class="text-gray-500">&copy; <span id="currentYear"></span> جميع الحقوق محفوظة.</p>
  </footer>

  <a href="https://wa.me/201027528761" target="_blank"
     id="fixed-whatsapp-button"
     class="inline-block bg-green-600 hover:bg-green-700 text-white font-bold py-3 px-6 rounded-full shadow-xl transition-transform transform hover:scale-105"
     aria-label="Chat with me on WhatsApp"> <svg xmlns="http://www.w3.org/2000/svg" class="inline-block h-6 w-6 mr-2" fill="currentColor" viewBox="0 0 24 24"><path d="M.057 24l1.687-6.163c-1.041-1.804-1.588-3.849-1.587-5.946.003-6.556 5.338-11.891 11.893-11.891 3.181.001 6.167 1.24 8.413 3.488 2.245 2.248 3.481 5.236 3.48 8.414-.003 6.557-5.338 11.892-11.893 11.892-1.99-.001-3.951-.5-5.688-1.448l-6.305 1.654zm6.597-3.807c1.676.995 3.276 1.591 5.392 1.592 5.448 0 9.886-4.434 9.889-9.885.002-5.462-4.415-9.89-9.881-9.892-5.452 0-9.887 4.434-9.889 9.884-.001 2.225.651 3.891 1.746 5.634l-.999 3.648 3.712-1.033z"/></svg>
    واتساب: 01027528761
  </a>

  <dialog id="copyright-dialog" class="p-6 rounded-lg shadow-xl backdrop-blur-md bg-black/70 text-white text-center rgb-border-animated" aria-modal="true" aria-labelledby="dialog-title" aria-describedby="dialog-description"> <p id="dialog-title" class="text-xl mb-4 font-bold glow-text">إشعار حقوق الملكية الفكرية</p> <p id="dialog-description" class="mb-6 leading-loose text-gray-300"> أنا علي عمري 15 مبرمج محترف.
      <br>
      <span class="text-yellow-400">أي محاولة لأخذ الأكواد في هذا الموقع وتدعي أنها ملكك،</span>
      <br>
      فسوف يتم التعامل مع الأمر بما يقتضيه القانون وحقوق الملكية الفكرية.
    </p>
    <button id="close-dialog" class="bg-blue-600 hover:bg-blue-700 text-white font-bold py-2 px-6 rounded-full transition" aria-label="Close dialog"> حسناً
    </button>
  </dialog>


  <script>
    // --- Advanced JS: Performance Monitoring & Global Error Handling ---

    // Mark the start of the script execution timing
    // This uses the High Resolution Time API, which is more precise than new Date().getTime()
    performance.mark('scriptStart');

    // Global Error Handling: Catch any uncaught errors on the window
    // This is a more robust way to log errors than just relying on the console
    window.onerror = function(message, source, lineno, colno, error) {
        console.error('*** Global Error Caught (Advanced JS Error Handling) ***');
        console.error('Message:', message);
        console.error('Source:', source); // The URL of the script where the error occurred
        console.error('Line:', lineno);     // The line number
        console.error('Column:', colno);   // The column number
        console.error('Error Object:', error); // The actual Error object, provides stack trace
        console.error('*******************************************************');
        // Returning true would suppress the default browser console logging.
        // Returning false (or omitting return) allows default logging as well.
        return false; // Allow default console logging
    };

    // --- Main Script Logic Wrapped in Try...Catch ---
    // This helps catch synchronous errors within this block and prevents them
    // from stopping the entire script, although window.onerror acts as a fallback.
    try {
        // --- Theme Toggle Script ---
        // Mark the start of DOMContentLoaded event handling
        performance.mark('DOMContentLoadedHandlerStart');
        document.addEventListener('DOMContentLoaded', function() {
            // Mark when the DOM is fully loaded and parsed
            performance.mark('DOMContentLoaded');

            const themeToggleBtn = document.getElementById('theme-toggle');
            const body = document.body;
            const lightModeClass = 'light-mode';
            const themePreference = localStorage.getItem('theme');

            if (themePreference === 'light') {
                body.classList.add(lightModeClass);
            } else {
                body.classList.remove(lightModeClass);
            }

            if (themeToggleBtn) {
               themeToggleBtn.addEventListener('click', function() {
                   body.classList.toggle(lightModeClass);
                   if (body.classList.contains(lightModeClass)) {
                       localStorage.setItem('theme', 'light');
                   } else {
                       localStorage.setItem('theme', 'dark');
                   }
               });
            }

             const currentYearSpan = document.getElementById('currentYear');
             if (currentYearSpan) {
                currentYearSpan.textContent = new Date().getFullYear();
             }

            // Mark the end of DOMContentLoaded event handling
            performance.mark('DOMContentLoadedHandlerEnd');
        });


        // --- Loader Script (Modified for minimum duration and Performance Marks) ---
        const minimumLoadTime = 3000; // Minimum 3 seconds display for loader
        let pageLoaded = false;
        let minimumTimePassed = false;

        function hideLoaderIfReady() {
            performance.mark('hideLoaderIfReadyStart'); // Mark start of hideLoaderIfReady function execution

            if (pageLoaded && minimumTimePassed) {
                const loader = document.getElementById('loader-overlay');
                if (loader) {
                  loader.classList.add('hidden');

                   performance.mark('ScrollRevealInitStart'); // Mark start of SR init
                   initializeScrollReveal();
                   performance.mark('ScrollRevealInitEnd'); // Mark end of SR init

                   // Reveal the fixed WhatsApp button - this uses the existing ScrollReveal instance
                   // Note: ScrollReveal can directly manage visibility and opacity.
                   ScrollReveal().reveal('#fixed-whatsapp-button', {
                        delay: 500, duration: 1000, distance: '50px', origin: 'bottom', easing: 'ease-out', opacity: 1, visibility: 'visible'
                   });

                   performance.mark('BasicSecurityCheckStart'); // Mark start of security check
                   basicClientSideSecurityCheck();
                   performance.mark('BasicSecurityCheckEnd'); // Mark end of security check

                   performance.mark('ShowCopyrightDialogStart'); // Mark start of dialog show logic
                   showCopyrightDialog();
                   // showCopyrightDialogEnd is marked inside the setTimeout of showCopyrightDialog


                   // --- Performance Measurements ---
                   // Calculate durations between marks and log them to the console
                   // Measures are created and stored, retrieve and log them later
                   performance.measure('1. Time to DOM Ready (Script Start -> DOMContent)', 'scriptStart', 'DOMContentLoaded');
                   performance.measure('2. DOMContentLoaded Handling Time', 'DOMContentLoadedHandlerStart', 'DOMContentLoadedHandlerEnd');
                   // ensure 'windowLoad' mark is set before measuring to it
                   if(performance.getEntriesByName('windowLoad').length > 0) {
                       performance.measure('3. Time to Window Load (Script Start -> window.load)', 'scriptStart', 'windowLoad');
                   } else {
                       console.warn("Performance measurement 'Time to Window Load' skipped: windowLoad mark not found.");
                   }
                   performance.measure('4. Minimum Loader Time Passed', 'scriptStart', 'minimumTimePassed');
                   performance.measure('5. hideLoaderIfReady Execution Time', 'hideLoaderIfReadyStart', 'hideLoaderIfReadyEnd');
                   performance.measure('6. ScrollReveal Initialization Time', 'ScrollRevealInitStart', 'ScrollRevealInitEnd');
                   performance.measure('7. Basic Security Check Time', 'BasicSecurityCheckStart', 'BasicSecurityCheckEnd');
                   // Vue App setup time is marked separately below

                   // Mark the point where all critical setup is done after loader hides
                   performance.mark('PageInteractive');


                   console.log('*** Performance Measurements (Advanced JS) ***');
                   // Get all recorded measurements
                   performance.getEntriesByType('measure').forEach(measure => {
                       console.log(`${measure.name}: ${measure.duration.toFixed(2)} ms`);
                   });

                   // Log Vue App setup time specifically
                   const vueSetupMeasure = performance.getEntriesByName('Vue App Setup Time')[0];
                   if (vueSetupMeasure) {
                       console.log(`${vueSetupMeasure.name}: ${vueSetupMeasure.duration.toFixed(2)} ms`);
                   }

                   console.log('********************************************');


                }
                 performance.mark('hideLoaderIfReadyEnd'); // Mark end of hideLoaderIfReady function execution
            }
        }

        // Event listener for when the entire page and all dependent resources (images, scripts, etc.) have loaded
        performance.mark('windowLoadHandlerStart'); // Mark start of window load handler
        window.addEventListener('load', function() {
            performance.mark('windowLoad'); // Mark window load event fired
            pageLoaded = true;
            hideLoaderIfReady();
            performance.mark('windowLoadHandlerEnd'); // Mark end of window load handler
        });

        // Set a timeout for the minimum display duration of the loader
        performance.mark('minimumTimeTimeoutSetup'); // Mark timeout setup
        setTimeout(function() {
            performance.mark('minimumTimePassed'); // Mark when minimum time timeout fires
            minimumTimePassed = true;
            hideLoaderIfReady();
        }, minimumLoadTime);


        // --- ScrollReveal Initialization ---
        function initializeScrollReveal() {
             // Removed performance marks from here as they are now in hideLoaderIfReady
            // ... (Existing ScrollReveal reveal calls) ...
            ScrollReveal().reveal('header h1, header p', { delay: 300, duration: 800, distance: '50px', origin: 'top', easing: 'ease-out' });
            ScrollReveal().reveal('#about h2, #about p', { delay: 200, duration: 800, distance: '50px', origin: 'left', easing: 'ease-out' });

           // Reveal the entire skills section container
           ScrollReveal().reveal('#skills', {
               delay: 200, duration: 800, distance: '50px', origin: 'bottom', easing: 'ease-out',
               afterReveal: function (domEl) {
                    // Add a class after the section is revealed by ScrollReveal
                    domEl.classList.add('is-revealed');
                    // Vue components inside will react to visibility via their own IntersectionObservers (for progress bars)
               }
            });

            ScrollReveal().reveal('#contact h2, #contact p, #contact a', { delay: 200, duration: 800, distance: '50px', origin: 'right', easing: 'ease-out', interval: 100 });

             // The fixed WhatsApp button is revealed separately in hideLoaderIfReady
        }


        // --- Basic Client-Side Security Check ---
        function basicClientSideSecurityCheck() {
            // Removed performance marks from here as they are now in hideLoaderIfReady
            // ... (Existing security check code) ...
             const urlParams = new URLSearchParams(window.location.search);
             const suspiciousPatterns = [
                 /<script>/i, /<\/script>/i, /['"]/g, /[<>]/g, /=/g,
                 /ONLOAD/i, /onerror/i, /javascript:/i, /<[^>]*>/
             ];
             let suspiciousFound = false; // Flag to indicate if any suspicious pattern was found

             for (const [key, value] of urlParams) {
                 const checkString = key + '=' + value;
                 for (const pattern of suspiciousPatterns) {
                     if (pattern.test(checkString)) {
                         console.warn( "*** تنبيه أمان (فحص أساسي من جانب المستخدم) ***\n" + "تم العثور على نمط يحتمل أن يكون مشبوهًا في معاملات عنوان URL:\n" + "المعامل (Key): " + key + "\n" + "القيمة (Value): " + value + "\n" + "النمط المطابق: " + pattern.source + "\n\n" + "ملاحظة: هذا مجرد فحص أساسي على جانب المستخدم ولا يضمن الأمان.\n" + "يجب تطبيق إجراءات أمان قوية على جانب الخادم.");
                         suspiciousFound = true; // Set flag
                         // Optional: break from inner loop or outer loop if one match is enough
                         // break;
                     }
                 }
                 // if (suspiciousFound) break; // Optional: break from outer loop
             }

             if (!suspiciousFound && urlParams.toString() !== "") {
                 // Optional: Log params if they exist but aren't suspicious, for debugging
                 // console.info("URL parameters checked: No suspicious patterns found.");
             } else if (urlParams.toString() === "") {
                  // Optional: Log if no params were present
                 // console.info("No URL parameters to check.");
             }
        }

        // --- Copyright Dialog Script (Using showModal for better native behavior) ---
        const copyrightDialog = document.getElementById('copyright-dialog');
        const closeDialogButton = document.getElementById('close-dialog');

        function showCopyrightDialog() {
             // Removed performance marks from here as they are now in hideLoaderIfReady
            setTimeout(() => {
                // Use showModal for native handling of modality and backdrop
                if (copyrightDialog && !copyrightDialog.open) {
                     copyrightDialog.showModal();
                     // The [open] attribute is added by showModal, triggering CSS animation
                }
            }, 100); // Short delay to ensure DOM is ready/layout stabilized
        }

        if (closeDialogButton) {
            closeDialogButton.addEventListener('click', function() {
                if (copyrightDialog && copyrightDialog.open) {
                   copyrightDialog.close(); // Use close() to close the dialog
                }
            });
        }

        // Allow closing dialog by clicking on the backdrop (more reliable check)
        if (copyrightDialog) {
            copyrightDialog.addEventListener('click', function(event) {
                // Check if the click target is the dialog element itself (the backdrop area)
                const rect = copyrightDialog.getBoundingClientRect();
                const isInDialog = (rect.top <= event.clientY && event.clientY <= rect.top + rect.height &&
                                    rect.left <= event.clientX && event.clientX <= rect.left + rect.width);
                if (!isInDialog && copyrightDialog.open) {
                    copyrightDialog.close();
                }
            });

             // Note: Escape key closing is usually handled natively by <dialog> when used with showModal()
        }


        // --- Vue.js Code for Skills Section (with requestAnimationFrame and corrected observer cleanup) ---
        // This part uses Vue's features which are already based on modern JS

        // Define the data for skills
        const skillsData = [
            { name: 'HTML', description: 'لغة ترميز أساسية لبناء هيكل ومحتوى صفحات الويب بشكل دلالي.', level: 95, color: 'orange-500' },
            { name: 'CSS', description: 'لتصميم وتنسيق مظهر صفحات الويب، مع دعم التجاوب والتخطيطات المعقدة.', level: 90, color: 'blue-500' },
            { name: 'JavaScript', description: 'لغة برمجة لجعل المواقع تفاعلية وديناميكية، تعمل في المتصفح والخادم.', level: 85, color: 'green-500' },
            { name: 'React.js', description: 'مكتبة JavaScript لبناء واجهات مستخدم حديثة ومرنة قائمة على المكونات.', level: 80, color: 'blue-600' },
            { name: 'Tailwind CSS', description: 'إطار عمل CSS لبناء تصميمات مخصصة بسرعة باستخدام فئات مساعدة.', level: 95, color: 'pink-600' },
            { name: 'TypeScript', description: 'يضيف نظام أنواع لـ JavaScript لتحسين جودة المشاريع الكبيرة واكتشاف الأخطاء مبكرًا.', level: 75, color: 'cyan-600' },
            { name: 'Vue.js', description: 'إطار عمل JavaScript تقدمي لبناء واجهات مستخدم مرنة وسهلة التعلم.', level: 60, color: 'emerald-600' },
            { name: 'Angular', description: 'منصة شاملة من جوجل لبناء تطبيقات ويب كبيرة أحادية الصفحة باستخدام TypeScript.', level: 50, color: 'red-600' },
            { name: 'Python', description: 'لغة برمجة متعددة الاستخدامات (ويب، بيانات، ذكاء اصطناعي) معروفة ببساطتها وقوتها.', level: 70, color: 'yellow-600' },
            { name: 'PHP', description: 'لغة برمجة نصية شائعة لتطوير الويب من جانب الخادم، أساس ووردبريس.', level: 65, color: 'indigo-600' },
            { name: 'Java', description: 'لغة قوية ومستقلة عن المنصة لتطبيقات الشركات الكبرى وتطبيقات أندرويد.', level: 55, color: 'orange-600' },
            { name: 'SQL', description: 'لغة استعلام قياسية لإدارة البيانات والتفاعل مع قواعد البيانات العلائقية.', level: 70, color: 'teal-600' },
            { name: 'Node.js', description: 'بيئة تشغيل JavaScript لتطوير تطبيقات خادم سريعة وفعالة وقابلة للتوسع.', level: 75, color: 'lime-600' },
        ];

        // Define the SkillCard component using Vue's Options API
        const SkillCard = {
            props: ['skill'],
            template: `
                <div class="bg-black/40 p-6 rounded-lg shadow-lg hover:scale-105 transition transform flex flex-col justify-start items-center min-h-[210px]">
                    <h3 :class="['text-xl font-bold mb-2', 'text-' + skill.color]">{{ skill.name }}</h3>
                    <p class="text-sm text-gray-300 leading-relaxed mb-4">{{ skill.description }}</p>
                    <div class="mt-auto w-full bg-gray-700 rounded-full h-2.5 dark:bg-gray-700"
                         role="progressbar"
                         :aria-valuenow="skill.level" aria-valuemin="0" aria-valuemax="100">
                        <div ref="progressBarInner"
                             :class="['h-2.5 rounded-full text-xs text-white text-center py-0.5', 'bg-' + skill.color]"
                             :style="{ width: barWidth + '%', transition: 'width 1s ease-in-out' }">
                             </div>
                    </div>
                </div>
            `,
            data() {
                return {
                    barWidth: 0, // Initial width state for animation
                    observer: null // Added property to store the observer instance
                };
            },
            // Lifecycle hook called after the component is mounted to the DOM
            mounted() {
                // Use Intersection Observer to trigger the animation when the component becomes visible
                this.observer = new IntersectionObserver( // Store observer in data
                    (entries) => {
                        entries.forEach(entry => {
                            // Check if the element is intersecting and the bar hasn't been animated yet
                            if (entry.isIntersecting && this.barWidth === 0) {
                                // Use requestAnimationFrame for smoother animation timing
                                requestAnimationFrame(() => {
                                     this.barWidth = this.skill.level; // Update data property to trigger transition
                                });
                                // Optional: Unobserve after the first animation if you don't need re-animation
                                // this.observer.unobserve(entry.target);
                            }
                        });
                    },
                    {
                        root: null, // Observe relative to the viewport
                        threshold: 0.5 // Trigger when 50% of the component is visible
                    }
                );

                // Observe the component's root element ($el)
                this.observer.observe(this.$el);
            },
             // Correct Vue 3 Options API hook called before component is unmounted
             beforeUnmount() { // This hook is called just before the component is removed
                  if (this.observer) {
                      this.observer.disconnect(); // Clean up the observer
                  }
             },
             // unmounted() is called after the component is removed. Cleanup should happen BEFORE.
             // unmounted() { console.log('SkillCard unmounted:', this.skill.name); }
        };

        // --- Vue App Initialization ---
        performance.mark('VueAppSetupStart'); // Mark start of Vue setup

        // Create the Vue application instance
        const app = Vue.createApp({
            data() {
                return {
                    skills: skillsData // Make skillsData available to the template
                };
            },
            components: {
                'skill-card': SkillCard // Register the custom element tag <skill-card>
            },
            mounted() {
                // This is the root app mounted hook
                performance.mark('VueAppMounted'); // Mark when the main Vue app is mounted
            }
        });

        // Mount the Vue app to the specified div in the HTML
        app.mount('#skills-app');

        // Mark the end of Vue setup logic execution
        performance.mark('VueAppSetupEnd');

        // --- Measure Vue App Setup Time ---
        performance.measure('8. Vue App Setup Time', 'VueAppSetupStart', 'VueAppSetupEnd');


    } catch (error) {
        // Catch any synchronous errors that occur during the execution of the main script block
        console.error('*** Local Error Caught (Advanced JS Try/Catch Block) ***');
        console.error('Error Details:', error);
        console.error('*******************************************************');
         // You could also display a user-friendly error message on the page here
         // const errorDiv = document.getElementById('error-message');
         // if (errorDiv) errorDiv.textContent = 'An error occurred loading the page.';
    }

    // Note: The `performance.measure` and `console.log` for performance metrics
    // are primarily called within the `hideLoaderIfReady` function after the
    // necessary `performance.mark` calls have been set up.

</script>
</body>
</html>
