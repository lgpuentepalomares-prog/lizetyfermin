<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Nuestra Boda - Fermín y Lizet</title>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Alex+Brush&family=Cormorant+Garamond:ital,wght@0,400;0,600;1,400&family=Montserrat:wght@300;400&display=swap" rel="stylesheet">
    
    <style>
        :root {
            --burgundy: #6b1d2f;
            --dark-burgundy: #4a121e;
            --gold: #c5a059;
            --cream: #fbf9f5;
            --charcoal: #2b2b2b;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            font-family: 'Cormorant Garamond', serif;
            background-color: var(--cream);
            color: var(--charcoal);
            text-align: center;
            overflow-x: hidden;
            min-height: 100vh;
        }

        /* -------------------------------------------
           PANTALLA DEL SOBRE INTERACTIVO
        ------------------------------------------- */
        #envelope-container {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100vh;
            background-color: var(--cream);
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            z-index: 1000;
            transition: opacity 1.2s ease, visibility 1.2s;
            perspective: 1000px;
        }

        .envelope-wrapper {
            position: relative;
            width: 300px;
            height: 200px;
            background-color: var(--burgundy);
            border-radius: 0 0 8px 8px;
            box-shadow: 0 15px 35px rgba(0, 0, 0, 0.2);
            cursor: pointer;
            margin-bottom: 30px;
            transform-style: preserve-3d;
            transition: transform 0.5s ease;
        }

        .envelope-wrapper:hover {
            transform: scale(1.03) translateY(-5px);
        }

        .flap {
            position: absolute;
            top: 0;
            left: 0;
            width: 0;
            height: 0;
            border-left: 150px solid transparent;
            border-right: 150px solid transparent;
            border-top: 110px solid var(--dark-burgundy);
            border-radius: 4px 4px 0 0;
            transform-origin: top;
            transition: transform 0.8s ease;
            z-index: 4;
        }

        .envelope-front-pockets {
            position: absolute;
            bottom: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(30deg, #5c1828 50%, transparent 50%),
                        linear-gradient(-30deg, #5c1828 50%, transparent 50%);
            z-index: 3;
            border-radius: 0 0 8px 8px;
        }

        .gold-seal {
            position: absolute;
            top: 90px;
            left: 130px;
            width: 42px;
            height: 42px;
            background: radial-gradient(circle, #e8cb8d 0%, #c5a059 70%, #9a7633 100%);
            border-radius: 46% 52% 48% 54% / 50% 46% 54% 48%;
            z-index: 5;
            box-shadow: 
                inset -2px -2px 4px rgba(0,0,0,0.4),
                inset 2px 2px 4px rgba(255,255,255,0.6),
                0 4px 10px rgba(0,0,0,0.3);
            transition: transform 0.6s cubic-bezier(0.175, 0.885, 0.32, 1.275);
        }

        .gold-seal::after {
            content: "L & F";
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            font-family: 'Cormorant Garamond', serif;
            font-size: 0.75rem;
            font-weight: bold;
            color: #634d22;
            text-shadow: 1px 1px 1px rgba(255,255,255,0.4);
            letter-spacing: 1px;
            opacity: 0.8;
        }

        .click-hint {
            font-size: 1.1rem;
            letter-spacing: 2px;
            text-transform: uppercase;
            color: var(--burgundy);
            animation: pulse 2s infinite;
        }

        @keyframes pulse {
            0% { opacity: 0.4; transform: scale(0.98); }
            50% { opacity: 1; transform: scale(1); }
            100% { opacity: 0.4; transform: scale(0.98); }
        }

        .open-envelope .flap {
            transform: rotateX(180deg);
            z-index: 1;
        }

        .open-envelope .gold-seal {
            transform: translate3d(0, -50px, 0) scale(0);
            opacity: 0;
        }

        /* -------------------------------------------
           CONTENIDO PRINCIPAL Y ESPACIADOS GENERALES
        ------------------------------------------- */
        #main-content {
            opacity: 0;
            visibility: hidden;
            transition: opacity 1.5s ease 0.6s;
            padding-bottom: 120px; /* Mayor espacio al final de la página */
            position: relative;
        }

        .botanical-decor {
            position: absolute;
            width: 140px;
            height: auto;
            opacity: 0.12;
            pointer-events: none;
            z-index: 0;
        }
        .top-left { top: 40px; left: 40px; }
        .bottom-right { bottom: 140px; right: 40px; transform: rotate(180deg); }

        body.revealed #envelope-container {
            opacity: 0;
            visibility: hidden;
            pointer-events: none;
        }

        body.revealed #main-content {
            opacity: 1;
            visibility: visible;
        }

        /* Secciones con un padding vertical más generoso y elegante */
        .hero {
            padding: 120px 20px 80px 20px;
            background: var(--cream);
            position: relative;
            z-index: 1;
        }

        .verse {
            font-style: italic;
            font-size: 1.35rem;
            max-width: 650px;
            margin: 0 auto 60px auto; /* Mayor separación con los nombres de los padres */
            color: #444;
            line-height: 1.7;
            padding: 0 15px;
        }

        .parents-section {
            font-size: 1.15rem;
            margin-bottom: 60px; /* Separación amplia hacia la frase de invitación */
            letter-spacing: 1px;
            line-height: 2;
        }

        .parents-title {
            font-size: 0.9rem;
            text-transform: uppercase;
            color: var(--gold);
            margin-bottom: 20px;
            letter-spacing: 2px;
            font-weight: 600;
        }

        /* Contenedor Destacado de Nombres */
        .wedding-names-container {
            margin: 50px auto 60px auto;
            max-width: 850px;
            padding: 0 15px;
        }

        .wedding-names-container h1 {
            font-family: 'Alex Brush', cursive;
            font-size: 5.5rem;
            color: var(--burgundy);
            font-weight: 400;
            line-height: 1.1;
            margin: 0;
            text-shadow: 1px 1px 1px rgba(0,0,0,0.02);
        }

        .name-connector {
            display: flex;
            align-items: center;
            justify-content: center;
            margin: 25px 0;
        }

        .name-connector span.line {
            flex: 1;
            max-width: 80px;
            height: 1px;
        }

        .name-connector span.text {
            font-family: 'Cormorant Garamond', serif;
            font-size: 2rem;
            font-style: italic;
            color: var(--gold);
            margin: 0 25px;
            font-weight: 400;
            font-variant: lowercase;
        }

        .date-display {
            font-size: 1.6rem;
            border-top: 1px solid var(--gold);
            border-bottom: 1px solid var(--gold);
            display: inline-block;
            padding: 16px 45px;
            margin: 40px 0;
            letter-spacing: 3px;
            text-transform: uppercase;
        }

        .countdown-container {
            margin: 50px 0 30px 0;
        }
        
        .countdown {
            display: flex;
            justify-content: center;
            gap: 20px;
            margin-top: 25px;
        }

        .time-box {
            background: white;
            padding: 18px 16px;
            border-radius: 6px;
            min-width: 85px;
            box-shadow: 0 8px 25px rgba(0,0,0,0.02);
            border: 1px solid rgba(197, 160, 89, 0.15);
        }

        .time-box span {
            display: block;
            font-size: 1.9rem;
            font-weight: 600;
            color: var(--burgundy);
        }

        .time-box label {
            display: block;
            font-size: 0.75rem;
            text-transform: uppercase;
            color: #777;
            letter-spacing: 1px;
            margin-top: 5px;
        }

        /* Carrusel de Fotos */
        .slideshow-section {
            padding: 80px 20px 100px 20px; /* Espaciado premium para aislar la galería */
            background: var(--cream);
            border-bottom: 1px solid rgba(197, 160, 89, 0.15);
        }

        .slideshow-container {
            max-width: 480px;
            position: relative;
            margin: 0 auto;
            height: 580px;
            border-radius: 8px;
            overflow: hidden;
            box-shadow: 0 15px 40px rgba(0,0,0,0.08);
            border: 6px solid white;
        }

        .mySlides {
            display: none;
            position: absolute;
            width: 100%;
            height: 100%;
        }

        .mySlides img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }

        .fade-effect {
            animation: fadeAnim 1.5s;
        }

        @keyframes fadeAnim {
            from { opacity: 0.4; } 
            to { opacity: 1; }
        }

        .prev, .next {
            cursor: pointer;
            position: absolute;
            top: 50%;
            width: auto;
            padding: 18px;
            margin-top: -24px;
            color: white;
            font-weight: bold;
            font-size: 20px;
            transition: 0.3s ease;
            border-radius: 0 4px 4px 0;
            user-select: none;
            background-color: rgba(0, 0, 0, 0.25);
            text-decoration: none;
            z-index: 10;
        }

        .prev { left: 0; border-radius: 0 4px 4px 0; }
        .next { right: 0; border-radius: 4px 0 0 4px; }

        .prev:hover, .next:hover {
            background-color: var(--burgundy);
        }

        .dot-container {
            text-align: center;
            margin-top: 25px;
        }

        .dot {
            cursor: pointer;
            height: 9px;
            width: 9px;
            margin: 0 6px;
            background-color: #ccc;
            border-radius: 50%;
            display: inline-block;
            transition: background-color 0.6s ease;
        }

        .active-dot, .dot:hover {
            background-color: var(--burgundy);
        }

        /* Detalles e Iglesia */
        .details {
            background-color: white;
            padding: 110px 20px; /* Sección limpia y muy bien espaciada */
            border-bottom: 1px solid rgba(197, 160, 89, 0.15);
            position: relative;
            z-index: 1;
        }

        .section-title {
            font-size: 2rem;
            color: var(--burgundy);
            margin-bottom: 50px;
            text-transform: uppercase;
            letter-spacing: 3px;
        }

        .event-info {
            font-size: 1.3rem;
            margin-bottom: 50px;
            line-height: 1.9;
        }

        .location-link {
            display: block;
            max-width: 480px;
            margin: 0 auto;
            padding: 30px 24px;
            background-color: var(--cream);
            border: 1px solid rgba(197, 160, 89, 0.25);
            border-radius: 8px;
            text-decoration: none;
            color: var(--charcoal);
            transition: all 0.3s ease;
            box-shadow: 0 4px 20px rgba(0,0,0,0.01);
        }

        .location-link:hover {
            transform: translateY(-4px);
            box-shadow: 0 12px 30px rgba(107, 29, 47, 0.06);
            border-color: var(--burgundy);
        }

        .church-name {
            font-weight: 600;
            color: var(--burgundy);
            letter-spacing: 1px;
            font-size: 1.5rem;
        }

        .address-text {
            font-size: 1.1rem;
            color: #555;
            margin-top: 8px;
        }

        .map-hint {
            display: inline-block;
            margin-top: 16px;
            font-size: 0.85rem;
            text-transform: uppercase;
            color: var(--gold);
            letter-spacing: 1.5px;
            font-weight: 600;
        }

        .schedule {
            margin: 55px 0;
            font-size: 1.2rem;
            line-height: 2.2;
        }

        .padrinos {
            margin-top: 60px;
            font-size: 1.3rem;
            line-height: 1.9;
        }

        .padrinos p:not(.parents-title) {
            margin-bottom: 6px;
        }

        /* Ajustes responsivos para pantallas pequeñas */
        @media (max-width: 600px) {
            .wedding-names-container h1 { font-size: 3.8rem; }
            .hero { padding-top: 90px; }
            .details { padding: 80px 20px; }
            .slideshow-section { padding: 60px 20px; }
            .slideshow-container { height: 440px; }
        }
    </style>
</head>
<body>

    <div id="envelope-container" onclick="openInvitation()">
        <div class="envelope-wrapper" id="envelope">
            <div class="flap"></div>
            <div class="envelope-front-pockets"></div>
            <div class="gold-seal"></div>
        </div>
        <p class="click-hint">Toca el sello de cera para abrir</p>
    </div>

    <main id="main-content">
        
        <svg class="botanical-decor top-left" viewBox="0 0 100 100">
            <path d="M10,90 Q30,60 50,20 T90,10 M50,20 Q40,30 30,25 Q45,15 50,20 M70,12 Q65,25 55,20 Q70,5 70,12" stroke="#c5a059" stroke-width="0.7" fill="none"/>
        </svg>
        <svg class="botanical-decor bottom-right" viewBox="0 0 100 100">
            <path d="M10,90 Q30,60 50,20 T90,10 M50,20 Q40,30 30,25 Q45,15 50,20 M70,12 Q65,25 55,20 Q70,5 70,12" stroke="#c5a059" stroke-width="0.7" fill="none"/>
        </svg>

        <section class="hero">
            <p class="verse">“Así que ya no son dos, sino uno solo. Por tanto, lo que Dios ha unido, que no lo separe el hombre”</p>
            
            <div class="parents-section">
                <p class="parents-title">Con la bendición de Dios y de nuestros padres</p>
                <p>Rafael Puente Ramírez  |  Ricardo Rodríguez Cano</p>
                <p>Josefina Palomares Zavala  |  Angelina Piña Piña</p>
            </div>

            <p style="font-family: 'Montserrat', sans-serif; font-size: 0.85rem; text-transform: uppercase; letter-spacing: 4px; color: #666; margin-bottom: 10px; font-weight: 300;">
                Tenemos el honor de invitarlos a celebrar nuestra boda
            </p>
            
            <div class="wedding-names-container">
                <h1>Fermín Rodríguez Piña</h1>
                <div class="name-connector">
                    <span class="line" style="background: linear-gradient(to left, var(--gold), transparent);"></span>
                    <span class="text">y</span>
                    <span class="line" style="background: linear-gradient(to right, var(--gold), transparent);"></span>
                </div>
                <h1>Lizet Puente Palomares</h1>
            </div>
            
            <div class="date-display">
                Sábado • 24 Octubre • 2026
            </div>

            <div class="countdown-container">
                <div class="countdown" id="countdown">
                    <div class="time-box"><span id="days">00</span><label>Días</label></div>
                    <div class="time-box"><span id="hours">00</span><label>Horas</label></div>
                    <div class="time-box"><span id="minutes">00</span><label>Min</label></div>
                    <div class="time-box"><span id="seconds">00</span><label>Seg</label></div>
                </div>
            </div>
        </section>

        <section class="slideshow-section">
            <div class="slideshow-container">
                <div class="mySlides fade-effect">
                    <img src="IMG_3767.jpg" alt="Fermín y Lizet 1">
                </div>
                <div class="mySlides fade-effect">
                    <img src="E07673AE-243E-402A-AE39-BBACBAA55829.jpg" alt="Fermín y Lizet 2">
                </div>
                <div class="mySlides fade-effect">
                    <img src="2C9F3D57-52EE-4CBC-BBBF-B794F0DDF743.jpg" alt="Fermín y Lizet 3">
                </div>
                <div class="mySlides fade-effect">
                    <img src="IMG_3758.jpg" alt="Fermín y Lizet 4">
                </div>
                <div class="mySlides fade-effect">
                    <img src="FullSizeRender.jpg" alt="Fermín y Lizet 5">
                </div>

                <a class="prev" onclick="plusSlides(-1)">❮</a>
                <a class="next" onclick="plusSlides(1)">❯</a>
            </div>

            <div class="dot-container">
                <span class="dot" onclick="currentSlide(1)"></span>
                <span class="dot" onclick="currentSlide(2)"></span>
                <span class="dot" onclick="currentSlide(3)"></span>
                <span class="dot" onclick="currentSlide(4)"></span>
                <span class="dot" onclick="currentSlide(5)"></span>
            </div>
        </section>

        <section class="details">
            <h2 class="section-title">Detalles del Evento</h2>
            <div class="event-info">
                
                <a href="https://www.google.com/maps/place/St+James+Catholic+Church/@38.55494,-121.7509779,758m/data=!3m2!1e3!4b1!4m6!3m5!1s0x808529a344a5c52b:0xb55a3f7cec611723!8m2!3d38.55494!4d-121.748403!16s%2Fg%2F1vd3w03w?entry=ttu&g_ep=EgoyMDI2MDUyNy4wIKXMDSoASAFQAw%3D%3D" target="_blank" class="location-link">
                    <p class="church-name">ST. JAMES CATHOLIC CHURCH</p>
                    <p class="address-text">1275 B St, Davis, CA 95616</p>
                    <span class="map-hint">📍 Ver Mapa y Direcciones</span>
                </a>
                
                <div class="schedule">
                    <p><strong>Misa:</strong> 12:00 PM</p>
                    <p><strong>Comida:</strong> 3:00 PM - 6:00 PM</p>
                    <p><strong>Baile:</strong> 6:00 PM - 10:00 PM</p>
                </div>
            </div>

            <div class="padrinos">
                <p class="parents-title">Padrinos de Velación</p>
                <p>Salvador Puente Ramírez</p>
                <p>Rocío Palomares Zavala</p>
            </div>
        </section>

    </main>

    <script>
        function openInvitation() {
            const envelope = document.getElementById('envelope');
            envelope.classList.add('open-envelope');
            
            setTimeout(() => {
                document.body.classList.add('revealed');
                window.scrollTo({ top: 0, behavior: 'smooth' });
                startSlideshow();
            }, 700);
        }

        // Lógica del Contador
        const targetDate = new Date("October 24, 2026 12:00:00").getTime();

        const timer = setInterval(function() {
            const now = new Date().getTime();
            const distance = targetDate - now;

            const days = Math.floor(distance / (1000 * 60 * 60 * 24));
            const hours = Math.floor((distance % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
            const minutes = Math.floor((distance % (1000 * 60 * 60)) / (1000 * 60));
            const seconds = Math.floor((distance % (1000 * 60)) / 1000);

            if (document.getElementById("days")) {
                document.getElementById("days").innerText = days < 10 ? "0" + days : days;
                document.getElementById("hours").innerText = hours < 10 ? "0" + hours : hours;
                document.getElementById("minutes").innerText = minutes < 10 ? "0" + minutes : minutes;
                document.getElementById("seconds").innerText = seconds < 10 ? "0" + seconds : seconds;
            }

            if (distance < 0) {
                clearInterval(timer);
                document.getElementById("countdown").innerHTML = "<h3 style='color: var(--burgundy); font-size: 1.5rem;'>¡Llegó el Gran Día!</h3>";
            }
        }, 1000);

        // Lógica del Slide Show
        let slideIndex = 1;
        let slideTimer;
        showSlides(slideIndex);

        function plusSlides(n) {
            clearInterval(slideTimer);
            showSlides(slideIndex += n);
            startSlideshow();
        }

        function currentSlide(n) {
            clearInterval(slideTimer);
            showSlides(slideIndex = n);
            startSlideshow();
        }

        function showSlides(n) {
            let i;
            let slides = document.getElementsByClassName("mySlides");
            let dots = document.getElementsByClassName("dot");
            if (slides.length === 0) return;
            
            if (n > slides.length) {slideIndex = 1}    
            if (n < 1) {slideIndex = slides.length}
            
            for (i = 0; i < slides.length; i++) {
                slides[i].style.display = "none";  
            }
            for (i = 0; i < dots.length; i++) {
                dots[i].className = dots[i].className.replace(" active-dot", "");
            }
            slides[slideIndex-1].style.display = "block";  
            dots[slideIndex-1].className += " active-dot";
        }

        function startSlideshow() {
            slideTimer = setInterval(function() {
                slideIndex++;
                showSlides(slideIndex);
            }, 3500);
        }
    </script>
</body>
</html>
