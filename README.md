<!DOCTYPE html>
<html lang="en">
<head>
    <link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@400;700&display=swap" rel="stylesheet">
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Thiwthat</title>
    <style>
        /* รีเซ็ต CSS สำหรับหน้าจอ */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        .content {
            display: none; /* ซ่อนเนื้อหาหลัก */
            min-height: calc(100vh - 200px);
        }
        /* อนิเมชั่นพื้นหลัง */
        .falling-squares-container {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            overflow: hidden;
            z-index: -1;
        }
        .square { /* สี่เหลี่ยม */
            position: absolute;
            width: 40px;
            height: 40px;
            background-color: #64c8c6; 
            top: -50px; /* เริ่มจากด้านบน */
            animation: fall linear infinite;
        }        
        @keyframes fall { /* การเคลื่อนไหวสี่เหลี่ยม */ 
            0% {
                transform: translateY(-50px);
                opacity: 0.5;
            }
            100% {
                transform: translateY(100vh);
                opacity: 0;
            }
        }
        .shape {
            position: absolute;
            transform: scale(0);
            animation: grow-shrink 1.5s ease-out forwards;
        }
        .triangle {
            width: 0;
            height: 0;
            border-left: 60px solid transparent;
            border-right: 60px solid transparent;
            border-bottom: 120px solid #ff8fa5;
        }
        .hexagon {
            width: 120px;
            height: 120px;
            background: #ff8fa5;
            clip-path: polygon(50% 0%, 100% 25%, 100% 75%, 50% 100%, 0% 75%, 0% 25%);
        }
        .Diamond {
            width: 120px;
            height: 120px;
            background: #ff8fa5;
        }
        .star {
            width: 150px;
            height: 150px;
            background-color: #ff8fa5;
            clip-path: polygon(50% 0%, 61% 35%, 98% 35%, 68% 57%, 79% 91%, 50% 70%, 21% 91%, 32% 57%, 2% 35%, 39% 35%);
        }

        @keyframes grow-shrink {
            0% {
            transform: scale(0);
            opacity: 1;
            }
            50% {
            transform: scale(1.5);
            }
            100% {
            transform: scale(0);
            opacity: 0;
            }
        }      

        .landing-page {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            color: rgb(0, 0, 0);
            flex-direction: column;
        }
        .start-button {
            padding: 80px 220px;
            font-size: 160px;
            border: 10px solid #000000; /* เพิ่มขอบ 3px ด้วยสีฟ้า */
            background-color: #ff8fa5; /* สีปุ่ม */
            color: rgb(0, 0, 0); /* สีตัวอักษร */
            border-radius: 60px;
            cursor: pointer;
            position: relative;
            overflow: hidden; /* ทำให้ไม่โชว์ส่วนที่เกินขอบปุ่ม */
            transition: transform 0.3s, background-color 0.3s; /* เปลี่ยนแค่ขนาดและพื้นหลัง */
            z-index: 1; /* เพิ่ม z-index เพื่อให้ตัวอักษรอยู่เหนือพื้นหลัง */
        }
        .start-button::before {
            content: '';
            position: absolute;
            top: 0;
            left: 100%;
            width: 100%;
            height: 100%;
            background-color: #64c8c6; 
            transition: left 1.8s ease; /* ให้สีเลื่อนไปจากขวาไปซ้าย */
            z-index: -1; /* ทำให้พื้นหลังอยู่ด้านหลังตัวอักษร */
        }
        .start-button:hover {
            transform: scale(1.); /* ขยายขนาดปุ่ม */
            animation: shake 0.5s ease-in-out infinite; /* เพิ่มการสั่น */
            color: rgb(255, 255, 255);
        }
        .start-button:hover::before {
            left: 0; /* ให้แถบสีเลื่อนมาทางซ้าย */
        }   

        @keyframes shake {
            0% { transform: translateX(0); }
            25% { transform: translateX(-5px); }
            50% { transform: translateX(5px); }
            75% { transform: translateX(-5px); }
            100% { transform: translateX(0); }
        }
        .overlay {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 0;
            background-color: #000;
            z-index: 5;
            transition: height 0.8s ease-in-out;
        }

        .overlay.active {
            height: 100vh;
        }

        /* จัดการส่วนของ Body */
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            height: auto;
            overflow: auto;
            font-family: Arial, sans-serif;
            color: #c9ceec;
            background-color: #edd8db;
            position: relative;
        }

        .header-title {
            position: absolute; 
            top: 0; 
            padding: 10px 20px; 
            font-family: 'Montserrat', sans-serif;
            color: #ffe7e7; /* สีตัวอักษร */
            width: 120%; /* ให้ปรับตามเนื้อหา */
            font-size: 44px; /* ขนาดตัวอักษร */
            font-weight: bold;
            display: none;
            opacity: 0;
            transform: translateX(100%); 
            animation: slideInRight 3.2s ease forwards; 
        }
        .header-title.hidden {
            animation: none; /* ไม่ให้แสดงซ้ำ */
        }
        @keyframes slideInRight {
            20% {
            transform: translateX(100%); /* เริ่มจากนอกหน้าจอ */
            background-color: #edd8db; /* เริ่มด้วยสีดำ */
            opacity: 0; /* โปร่งใส */
        }
            80% {
            background-color: #ff8fa5; /* เปลี่ยนเป็นสีฟ้าตอนกลาง */
            opacity: 1; /* แสดงเต็ม */
        }
            100% {
            transform: translateX(0); /* มาถึงตำแหน่งสุดท้าย */
            background-color: #000000; /* คงสีฟ้าไว้ */
            opacity: 1;
        }}

        h2 {
            font-family: 'Montserrat', sans-serif;
            color: black;
            border: 5px solid #000000;
            font-weight: 700;
            font-size: 54px;
            margin-top: 140px;
            text-align: center;
        }
        h4 {
            font-family: 'Montserrat', sans-serif;
            color: black;
            border: 5px solid #000000;
            font-weight: 700;
            font-size: 60px;
            text-align: center;
        }

        .slideshow-container {
            max-width: 1200px;
            position: relative;
            margin: auto;
            overflow: hidden;
            border-radius: 10px;
            display: flex;
            align-items: center;
        }
        .mySlides {
            display: none;
            opacity: 0;
            transition: opacity 1s ease-in-out, transform 1s ease-in-out;
            flex: 1;
        }
        .active {
            display: block;
            opacity: 1;
            transform: translateY(0);
        }
        .fade-in {
            animation: fadeIn 1s forwards;
        }
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(-20px); }
            to { opacity: 1; transform: translateY(0); }
        }
        img {
            width: 100%;
            height: auto;
            margin-top: 10px;
            border: 4px solid #000000;
        }
        .prev, .next {
            cursor: pointer;
            padding: 20px;
            color: white;
            font-weight: bold;
            font-size: 150px;
            background-color: #e87d93;
            border: 4px solid #000000;
            border-radius: 40px;
            user-select: none;
            transition: background-color 0.3s, transform 0.3s;
            margin: 0 10px;
        }
        .prev:hover, .next:hover {
            background-color: #64c8c6;
            transform: scale(1.15);
        }

        .box-container {
            display: flex;
            justify-content: center;
            flex-wrap: wrap;
            margin: 30px;
        }
        .box {
            background-color: #e87d93;
            border: 4px solid #000000;
            border-radius: 10px;
            padding: 20px;
            margin: 140px 30px 0px 30px;
            width: 290px;
            height: 340px;
            box-shadow: 0 10px 10px rgba(0, 0, 0, 0.2);
            transition: transform 0.3s ease, background-color 0.3s ease;
            position: relative;
            overflow: hidden; /* ซ่อนส่วนที่ล้นออกจากขอบ */
            text-decoration: none; /* ไม่ให้มีเส้นใต้ */
        }
        .box::before {
            content: '';
            position: absolute;
            top: 0;
            left: 100%;
            width: 100%; height: 100%;
            background-color: #64c8c6; 
            transition: left 0.2s ease;
            z-index: 1; /* อยู่ใต้เนื้อหา */
        }
        .box:hover {
            transform: translateY(-5px) scale(1.1); 
        }
        .box:hover::before {
            left: 0; 
        }
        .box img {
            width: 75%;
            margin: 2px 2px 10px 30px;
            border-radius: 1px;
            z-index: 2; 
            position: relative;
        }
        .box-text { 
            font-size: 28px;
            color: #000000;
            text-align: center;
            margin-top: 2px;

        }

        footer {
            display: flex; 
            justify-content: space-between; /* กระจายส่วนต่างๆ*/
            align-items: center;
            height: 300px;
            background-color: #252525; 
            color: #fff; /* สีข้อความ */
            width: 100vw;
            margin-top: 160px;
            padding: 20px; /* เพิ่มระยะห่างภายใน */
        }           
        .footer-section {
            width: 33%;
            text-align: center; 
            border-right: 3px solid #fff; /* เส้นขวา */
            height: 80%;
        }.footer-section:last-child {
            border-right: none; /* เอาเส้นขวาออก */
        }
        .footer-section h3 {
            margin-top: 0;
            color: #e87d93; /* สีหัวข้อ */
            font-size: 28px;
        }
        .footer-section p {
            margin: 40px 5px 5px 40px ;
            text-align: left; /* ให้ข้อความชิดซ้าย */
            font-size: 18px;
        }
        a {
            color: #64c8c6;  /* เปลี่ยนสีลิงก์เป็นสีฟ้า */
            text-decoration: none;  /* ไม่ให้มีเส้นใต้ */
        }
        a:hover {
            color: #e87d93;  /* เปลี่ยนสีลิงก์เมื่อเมาส์ชี้ */
            text-decoration: underline;  /* เพิ่มเส้นใต้เมื่อเมาส์ชี้ */
        }
        
    </style>
</head>
<body>
    <div class="falling-squares-container">
        <!-- สี่เหลี่ยม -->
        <div class="square" style="left: 10%; animation-duration: 1s; animation-delay: 0s;"></div>
        <div class="square" style="left: 55%; animation-duration: 2s; animation-delay: 1s;"></div>
        <div class="square" style="left: 30%; animation-duration: 3s; animation-delay: 2s;"></div>
        <div class="square" style="left: 70%; animation-duration: 2s; animation-delay: 1.5s;"></div>
        <div class="square" style="left: 20%; animation-duration: 3s; animation-delay: 0.5s;"></div>
        <div class="square" style="left: 90%; animation-duration: 6s; animation-delay: 1.5s;"></div>
        <div class="square" style="left: 15%; animation-duration: 2s; animation-delay: 0.5s;"></div>
        <div class="square" style="left: 80%; animation-duration: 1s; animation-delay: 0.5s;"></div>
    </div>

    <div class="landing-page">
        <h4>ยินดีต้อนรับ !!!</h4>
        <button class="start-button" onclick="enterWebsite()">เข้าสู่เว็ปไซต์</button>
        <div class="overlay"></div>
    </div>

    <!-- เนื้อหาหลัก -->
    <header>
        <div class="header-title">
            <h1>   แนะนำมหาวิทยาลัย</h1>
            
        </div>
    </header>
    <div class="content" id="main-content">
        <div class="header">
            <h1>.</h1>
        </div>

        <h2>มหาวิทยาลัยที่ยอดนิยมในปี 2024</h2>

        <div class="slideshow-container"> <!-- รูปสไลด์ -->
            <button class="prev" onclick="plusSlides(-1)">&#10094;</button>
            
            <div class="mySlides active">
                <img src="KU.p.png" alt="Image 1">
            </div>

            <div class="mySlides">
                <img src="CU.p.png" alt="Image 2">
            </div>

            <div class="mySlides">
                <img src="CMU.p.png" alt="Image 3">
            </div>

            <div class="mySlides">
                <img src="MU.p.png" alt="Image 4">
            </div>

            <div class="mySlides">
                <img src="KKU.p.png" alt="Image 5">
            </div>

            <div class="mySlides">
                <img src="TU.p.png" alt="Image 5">
            </div>

            <div class="mySlides">
                <img src="Uni.png" alt="Image 5">
            </div>

            <button class="next" onclick="plusSlides(1)">&#10095;</button>
        </div>

        <h2>สนใจเรื่องอะไร ???</h2>
        
        <div class="box-container"> <!-- ปุ่ม -->
            <a href="file:///D:/ProJect%2036/ProJect%2036/ProJect%2036/%E0%B8%A7%E0%B9%88%E0%B8%B2%E0%B8%87.html" target="_blank">
                <div class="box">
                    <img src="North.png" alt="Box Image 1">
                </div>
                <div class="box-text">มหาวิทยาลัยภาคเหนือ</div>
            </a>
            <a href="https://example.com/center" target="_blank">
                <div class="box">
                    <img src="center.png" alt="Box Image 2">
                </div>
                <div class="box-text">มหาวิทยาลัยภาคกลาง</div>
            </a>
            <a href="https://example.com/tcas" target="_blank">
                <div class="box">
                    <img src="Tcas.png" alt="Box Image 3">
                </div>
                <div class="box-text">แนะนำวิธีการ<br>สมัครมหาวิทยาลัย</div>
            </a>
            <a href="https://example.com/southeast" target="_blank">
                <div class="box">
                    <img src="Southeast.png" alt="Box Image 4">
                </div>
                <div class="box-text">มหาวิทยาลัย<br>ภาคตะวันออกเฉียงเหนือ</div>
            </a>
            <a href="https://example.com/south" target="_blank">
                <div class="box">
                    <img src="South.png" alt="Box Image 5">
                </div>
                <div class="box-text">มหาวิทยาลัยภาคใต้</div>
            </a>
        </div>
        <footer class="footer"> <!-- ด้านล่าง -->
            <div class="footer-section">
                <h3>ข้อมูลเกี่ยวกับเว็บไซต์</h3>
                <p>เว็ปไซต์สำหรับแนะนำมหาวิทยาลัยและแนะนำการสมัครเข้ามหาวิทยาลัย <br><br>หากสนใจหัวข้อใดสามารถกดเข้าเพื่อดูเนื้อหาข้อมูลที่คุณสนใจเพิ่มเติม</p>
            </div>
            <div class="footer-section">
                <h3>แหล่งข้อมูลอ้างอิง</h3>
                <p>แหล่งข้อมูลอ้างอิงของเนื้อหา<br><br> 
                    <a href="https://www.mytcas.com/" target="_blank">https://www.mytcas.com/</a><br><br>
                    <a href="https://www.opendurian.com/news/Uni_FoE/" target="_blank">https://www.opendurian.com/news/Uni_FoE/</a><br><br>
                    <a href="https://www.dek-d.com/tcas/" target="_blank">https://www.dek-d.com/tcas/</a></p>
            </div>
            <div class="footer-section">
                <h3>ข้อมูลติดต่อ</h3>
                <p>Thiwthat.chanthakarm@e-tech.ac.th <br><br>099-099-xxxx <br><br> IG: tthiwthat</p>
            </div>
        </footer>
    </div>
    
    <script>
    // สร้างรูปร่างเมื่อคลิกที่ส่วนอื่นๆ ของหน้าจอ
    document.addEventListener("click", (event) => {
    // ตรวจสอบว่าไม่ได้คลิกที่ปุ่ม
    if (event.target.tagName !== "BUTTON") {
    const shape = document.createElement("div");
    shape.classList.add("shape");

    // สุ่มรูปร่าง
    const shapes = ["triangle", "hexagon","Diamond","star"];
    shape.classList.add(shapes[Math.floor(Math.random() * shapes.length)]);

    const shapeSize = 50; // ตัวอย่างขนาดรูปร่าง 50x50 px
    const x = event.pageX - shapeSize / 1;
    const y = event.pageY - shapeSize / 1;
    shape.style.left = `${x}px`;
    shape.style.top = `${y}px`;

    document.body.appendChild(shape);

    // ลบรูปร่างเมื่ออนิเมชันเสร็จ
    setTimeout(() => {
      shape.remove();
    }, 1500);
    }
    });
        // ฟังก์ชันเปลี่ยนภาพในสไลด์
        let slideIndex = 0;
        showSlides();

        function showSlides() {
        const slides = document.getElementsByClassName("mySlides");
        for (let i = 0; i < slides.length; i++) {
            slides[i].classList.remove("active");
        }
        slideIndex++;
        if (slideIndex >= slides.length) { slideIndex = 0 }
        slides[slideIndex].classList.add("active");
        slides[slideIndex].classList.add("fade-in");
        setTimeout(showSlides, 6000);
        }

        // ฟังก์ชันเปลี่ยนภาพด้วยปุ่ม
        function plusSlides(n) {
        const slides = document.getElementsByClassName("mySlides");
        slides[slideIndex].classList.remove("active");
        slideIndex += n;
        if (slideIndex >= slides.length) { slideIndex = 0 }
        if (slideIndex < 0) { slideIndex = slides.length - 1 }
        slides[slideIndex].classList.add("active");
        slides[slideIndex].classList.add("fade-in");
        }




    function enterWebsite() {
        const header = document.querySelector('.header-title');
        header.style.display = 'block';
        // เพื่อให้สีดำเลื่อนขึ้น
        const overlay = document.querySelector('.overlay');
        overlay.classList.add('active');
        
        // หลังจากสีดำ ให้โหลดเนื้อหาหลัก
        setTimeout(function() {
            document.querySelector('.landing-page').style.display = 'none';  // ซ่อนหน้า
            document.getElementById('main-content').style.display = 'block';  // แสดงเนื้อหาหลัก
        }, 1500);  // เวลาที่ต้องรอก่อนโหลดหน้าใหม่
    }
        document.getElementById('enter-button').addEventListener('click', function () {
        const header = document.querySelector('.header-title');
        header.style.display = 'block'; // แสดง Header
        header.classList.add('animate'); // เปิดใช้งานอนิเมชัน
        document.querySelector('.landing-page').style.display = 'none'; // ซ่อนหน้า
    });
    
    </script>

</body>
</html>
