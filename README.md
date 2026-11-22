<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />

  <title>3D Profile Card</title>

  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    body {
      min-height: 100vh;
      display: flex;
      align-items: center;
      justify-content: center;

      background:
        radial-gradient(circle at 50% 40%, #202020 0%, #0b0b0b 45%, #050505 100%);

      font-family: Arial, Helvetica, sans-serif;
      overflow: hidden;
      perspective: 1200px;
    }

    /* Background glow */

    .glow {
      position: absolute;
      width: 450px;
      height: 450px;

      background: rgba(0, 150, 255, 0.15);

      filter: blur(100px);
      border-radius: 50%;

      animation: glowMove 5s ease-in-out infinite alternate;
    }

    @keyframes glowMove {
      from {
        transform: translate(-40px, -20px);
      }

      to {
        transform: translate(40px, 20px);
      }
    }

    /* Main 3D container */

    .scene {
      width: 330px;
      height: 430px;

      perspective: 1200px;

      position: relative;
      z-index: 2;
    }

    .card {
      width: 100%;
      height: 100%;

      position: relative;

      transform-style: preserve-3d;

      border-radius: 28px;

      background: rgba(20, 20, 20, 0.75);

      border: 1px solid rgba(255, 255, 255, 0.12);

      box-shadow:
        0 30px 80px rgba(0, 0, 0, 0.7),
        inset 0 0 30px rgba(255, 255, 255, 0.03);

      transition:
        transform 0.15s ease-out,
        box-shadow 0.3s ease;

      overflow: hidden;
    }

    /* Animated border */

    .card::before {
      content: "";

      position: absolute;
      inset: -2px;

      background:
        conic-gradient(
          from 0deg,
          transparent,
          #00aaff,
          transparent,
          #8b5cf6,
          transparent
        );

      animation: rotateBorder 5s linear infinite;

      z-index: -2;
    }

    .card::after {
      content: "";

      position: absolute;
      inset: 2px;

      border-radius: 26px;

      background: #111;

      z-index: -1;
    }

    @keyframes rotateBorder {
      from {
        transform: rotate(0deg);
      }

      to {
        transform: rotate(360deg);
      }
    }

    /* Profile image */

    .profile-container {
      position: absolute;

      top: 40px;
      left: 50%;

      transform:
        translateX(-50%)
        translateZ(70px);

      width: 190px;
      height: 190px;

      border-radius: 50%;

      padding: 5px;

      background:
        linear-gradient(
          135deg,
          #00c6ff,
          #0072ff,
          #8b5cf6
        );

      box-shadow:
        0 0 35px rgba(0, 153, 255, 0.35);

      animation: float 3s ease-in-out infinite;
    }

    @keyframes float {
      0%, 100% {
        transform:
          translateX(-50%)
          translateZ(70px)
          translateY(0);
      }

      50% {
        transform:
          translateX(-50%)
          translateZ(70px)
          translateY(-8px);
      }
    }

    .profile-container img {
      width: 100%;
      height: 100%;

      object-fit: cover;

      border-radius: 50%;

      display: block;

      border: 3px solid #111;
    }

    /* Name */

    .content {
      position: absolute;

      bottom: 42px;
      width: 100%;

      text-align: center;

      transform: translateZ(60px);
    }

    .content h1 {
      font-size: 28px;

      color: white;

      letter-spacing: 1px;

      text-shadow:
        0 0 20px rgba(0, 153, 255, 0.4);
    }

    .content p {
      margin-top: 8px;

      color: #aaa;

      font-size: 14px;

      letter-spacing: 2px;

      text-transform: uppercase;
    }

    /* Floating particles */

    .particle {
      position: absolute;

      width: 5px;
      height: 5px;

      background: #00aaff;

      border-radius: 50%;

      box-shadow:
        0 0 15px #00aaff;

      animation: particleFloat 4s infinite ease-in-out;
    }

    .p1 {
      top: 10%;
      left: 10%;
      animation-delay: 0s;
    }

    .p2 {
      top: 20%;
      right: 15%;
      animation-delay: 1s;
    }

    .p3 {
      bottom: 20%;
      left: 15%;
      animation-delay: 2s;
    }

    .p4 {
      bottom: 10%;
      right: 10%;
      animation-delay: 3s;
    }

    @keyframes particleFloat {
      0%, 100% {
        transform: translateY(0) scale(1);
        opacity: 0.4;
      }

      50% {
        transform: translateY(-25px) scale(1.5);
        opacity: 1;
      }
    }

    /* Mobile */

    @media (max-width: 500px) {

      .scene {
        width: 290px;
        height: 390px;
      }

      .profile-container {
        width: 165px;
        height: 165px;
      }

      .content h1 {
        font-size: 24px;
      }
    }
  </style>
</head>

<body>

  <div class="glow"></div>

  <div class="scene">

    <div class="card" id="card">

      <!-- Particles -->

      <span class="particle p1"></span>
      <span class="particle p2"></span>
      <span class="particle p3"></span>
      <span class="particle p4"></span>

      <!-- Profile -->

      <div class="profile-container">
        <img src="assets/profile.png" alt="Profile Photo">
      </div>

      <!-- Text -->

      <div class="content">
        <h1>Arpan Maji</h1>

        <p>Full Stack Developer</p>
      </div>

    </div>

  </div>


  <script>

    const card = document.getElementById("card");

    const scene = document.querySelector(".scene");

    scene.addEventListener("mousemove", (e) => {

      const rect = scene.getBoundingClientRect();

      const x = e.clientX - rect.left;
      const y = e.clientY - rect.top;

      const centerX = rect.width / 2;
      const centerY = rect.height / 2;

      const rotateX =
        ((y - centerY) / centerY) * -12;

      const rotateY =
        ((x - centerX) / centerX) * 12;

      card.style.transform = `
        rotateX(${rotateX}deg)
        rotateY(${rotateY}deg)
        scale3d(1.03, 1.03, 1.03)
      `;

    });


    scene.addEventListener("mouseleave", () => {

      card.style.transform = `
        rotateX(0deg)
        rotateY(0deg)
        scale3d(1, 1, 1)
      `;

    });

  </script>

</body>
</html>





<p align="center">
  <img src="https://readme-typing-svg.demolab.com?font=Fira+Code&size=25&pause=1000&color=36BCF7&center=true&vCenter=true&width=600&lines=Full+Stack+Web+Developer;Building+Modern+Web+Applications;Turning+Ideas+Into+Digital+Experiences;Always+Learning+%26+Building" alt="Typing SVG" />
</p>

<p align="center">
  <img src="https://komarev.com/ghpvc/?username=arpan61932&label=Profile%20Views&color=36BCF7&style=flat" alt="Profile Views" />
</p>

# 💫 About Me:
👋 Hi, I'm Arpan Maji<br><br>💻 Full Stack Web Developer focused on building scalable, responsive, and user-friendly web applications.<br><br>🚀 Skilled in React, JavaScript, Node.js, Express.js, MongoDB, and modern web technologies.<br><br>🛠️ I enjoy turning ideas into functional products and building clean, efficient, and engaging web experiences.<br><br>🌱 Currently expanding my skills through real-world projects and continuous learning.<br><br>🤝 Open to collaboration, interesting projects, and new opportunities.


## 🌐 Socials:
[![Discord](https://img.shields.io/badge/Discord-%237289DA.svg?logo=discord&logoColor=white)](https://discord.gg/https://discord.gg/wGjewWCWz) [![Instagram](https://img.shields.io/badge/Instagram-%23E4405F.svg?logo=Instagram&logoColor=white)](https://instagram.com/ar.pan47) [![LinkedIn](https://img.shields.io/badge/LinkedIn-%230077B5.svg?logo=linkedin&logoColor=white)](https://linkedin.com/in/arpan-maji) [![email](https://img.shields.io/badge/Email-D14836?logo=gmail&logoColor=white)](mailto:arpanmaji48@gmail.com) 

# 💻 Tech Stack:
![C](https://img.shields.io/badge/c-%2300599C.svg?style=for-the-badge&logo=c&logoColor=white) ![C++](https://img.shields.io/badge/c++-%2300599C.svg?style=for-the-badge&logo=c%2B%2B&logoColor=white) ![CSS3](https://img.shields.io/badge/css3-%231572B6.svg?style=for-the-badge&logo=css3&logoColor=white) ![HTML5](https://img.shields.io/badge/html5-%23E34F26.svg?style=for-the-badge&logo=html5&logoColor=white) ![JavaScript](https://img.shields.io/badge/javascript-%23323330.svg?style=for-the-badge&logo=javascript&logoColor=%23F7DF1E) ![Cloudflare](https://img.shields.io/badge/Cloudflare-F38020?style=for-the-badge&logo=Cloudflare&logoColor=white) ![Render](https://img.shields.io/badge/Render-%46E3B7.svg?style=for-the-badge&logo=render&logoColor=white) ![Vercel](https://img.shields.io/badge/vercel-%23000000.svg?style=for-the-badge&logo=vercel&logoColor=white) ![JWT](https://img.shields.io/badge/JWT-black?style=for-the-badge&logo=JSON%20web%20tokens) ![NodeJS](https://img.shields.io/badge/node.js-6DA55F?style=for-the-badge&logo=node.js&logoColor=white) ![Nodemon](https://img.shields.io/badge/NODEMON-%23323330.svg?style=for-the-badge&logo=nodemon&logoColor=%BBDEAD) ![React](https://img.shields.io/badge/react-%2320232a.svg?style=for-the-badge&logo=react&logoColor=%2361DAFB) ![React Hook Form](https://img.shields.io/badge/React%20Hook%20Form-%23EC5990.svg?style=for-the-badge&logo=reacthookform&logoColor=white) ![React Router](https://img.shields.io/badge/React_Router-CA4245?style=for-the-badge&logo=react-router&logoColor=white) ![TailwindCSS](https://img.shields.io/badge/tailwindcss-%2338B2AC.svg?style=for-the-badge&logo=tailwind-css&logoColor=white) ![Vite](https://img.shields.io/badge/vite-%23646CFF.svg?style=for-the-badge&logo=vite&logoColor=white) ![MongoDB](https://img.shields.io/badge/MongoDB-%234ea94b.svg?style=for-the-badge&logo=mongodb&logoColor=white) ![Supabase](https://img.shields.io/badge/Supabase-3ECF8E?style=for-the-badge&logo=supabase&logoColor=white) ![Postman](https://img.shields.io/badge/Postman-FF6C37?style=for-the-badge&logo=postman&logoColor=white)
# 📊 GitHub Stats:
![](https://github-readme-stats.shion.dev/api?username=arpan61932&theme=dark&hide_border=false&include_all_commits=false&count_private=false)<br/>
![](https://streak-stats.demolab.com/?user=arpan61932&theme=dark&hide_border=false)<br/>
![](https://github-readme-stats.shion.dev/api/top-langs/?username=arpan61932&theme=dark&hide_border=false&include_all_commits=false&count_private=false&layout=compact)

### 🔝 Top Contributed Repo
![](https://github-contributor-stats.vercel.app/api?username=arpan61932&limit=5&theme=dark&combine_all_yearly_contributions=true)

---
[![](https://komarev.com/ghpvc/?username=arpan61932&icon=0&color=0)](https://visitcount.itsvg.in)

<!-- Proudly created with GPRM ( https://gprm.itsvg.in ) -->
