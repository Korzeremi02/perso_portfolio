<script>
    import Logo from '../assets/logo.png';
    import Wp from '../assets/Wp/wp.jpg';
    import Xp from '../assets/Wp/xp.jpg';
    import { onMount, afterUpdate } from 'svelte';
    function getCurrentTime() {
        var now = new Date();
        var hours = now.getHours();
        var minutes = now.getMinutes();
        var secondes = now.getSeconds();
        hours = (hours < 10) ? "0" + hours : hours;
        minutes = (minutes < 10) ? "0" + minutes : minutes;
        secondes = (secondes < 10) ? "0" + secondes : secondes;
        var currentTime = hours + ":" + minutes + ":" + secondes;
        return currentTime;
    }
    var currentTime = getCurrentTime();
    function getCurrentDate() {
        var now = new Date();
        var year = now.getFullYear();
        var month = (now.getMonth() + 1); 
        var day = now.getDate();
        month = (month < 10) ? "0" + month : month;
        day = (day < 10) ? "0" + day : day;
        var daysOfWeek = ["Dimanche", "Lundi", "Mardi", "Mercredi", "Jeudi", "Vendredi", "Samedi"];
        var dayOfWeek = daysOfWeek[now.getDay()];
        var currentDate = dayOfWeek + " " + day + "/" + month + "/" + year;
        return currentDate;
    }
    var currentDate = getCurrentDate();
    function updateTime() {
        currentTime = getCurrentTime();
    }
    let currentImage = Wp;
    let images = [Wp, Xp];
    let currentIndex = 0;
    function nextImage() {
        currentIndex = (currentIndex + 1) % images.length;
        currentImage = images[currentIndex];
    }

    onMount(() => {
        setInterval(updateTime, 1000);
        setInterval(nextImage, 30000);
    });
    afterUpdate(() => {
        document.querySelector('.RacWp').style.opacity = 0;
        setTimeout(() => {
        document.querySelector('.RacWp').style.opacity = 1;
        }, 0);
    });
</script>

<style>
     @import '../public/global.css';
     /* App */
    .App { height: 100vh; width: 100vw; background-color: var(--color-background) ; overflow: hidden; }
    /* Wallpaper */
    .RacWp { position: absolute; height: 100vh; width: 100vw; object-fit: cover; object-position: center; transition: opacity 1s ease-in-out; }
    /* User interface area */
    .RacScreen { display: flex; flex-direction: column; align-items: center; justify-content: center; position: absolute; height: 100vh; width: 100vw; }
    /* MenuBar */
    .RacMenu > * { font-family: -apple-system, BlinkMacSystemFont, sans-serif; font-size: 0.9rem; color: #000; }
    .RacMenu { height: 3vh ; width: 100vw; background-color: #fff; opacity: 0.5; display: flex; align-items: center; }
    .RacLogo { margin-left: 8px ; height: 100%; width: fit-content; display: flex; align-items: center; justify-content: center; }
    .RacLogoImg { height: 100%; width: auto;}
    .RacMenuItems { display: flex; height: auto; width: fit-content; }
    .MenuFileSct{ height:100%; width: fit-content; margin: 0% 0% 0% 5%; }
    .MenuEditSct{ height:100%; width: fit-content; margin: 0% 5%; }
    .MenuViewSct{ height:100%; width: fit-content; margin: 0% 0%; }
    .MenuGoSct{ height:100%; width: fit-content; margin: 0% 5%; }
    .MenuWindowSct{ height:100%; width: fit-content; margin: 0% 0%; }
    .MenuHelpSct{ height:100%; width: fit-content; margin: 0% 5%; }
    .RacEmpty { flex-grow: 1; }
    .RacTime { display: flex; align-items: center;}
    .RacIcons { height: auto; width: fit-content; }
    .RacTimeText { height: 100%; width: fit-content; display: flex; align-items: center; ; margin-right: 5px; }
    .RacTimeTextHour { height: auto; width: fit-content; margin-right: 5px; }
    .RacTimeTextDate { height: auto; width: fit-content; margin-right: 5px; }
    /* Contents */
    .RacWindow { height: 91vh; width: 100vw; }
    .WindowTest { position: absolute; height: 70%; width: 60%; background-color: red; opacity: 0.5; backdrop-filter: blur(2px); visibility: hidden; }
    /* DockBar */
    @keyframes ZoomInDock { 0% { transform: scale(1); margin-bottom: 0; } 100% { transform: scale(1.33); margin-bottom: 1vh; }}
    @keyframes ZoomOutDock { 0% { transform: scale(1.33); margin-bottom: 1.15vh; } 50% { transform: scale(1.33); margin-bottom: 1.15vh; } 100% { transform: scale(1); margin-bottom: 0; }}
    .RacDock { animation-name: ZoomOutDock; animation-duration: 0.5s; animation-delay: 0s; height: 6vh; width: fit-content; display:flex; align-items: center; background-color: #fff; opacity: 0.5; backdrop-filter: blur(2px); border-radius: 10px 10px 0px 0px; }
    .RacDock:hover { animation-name: ZoomInDock; animation-duration: 0.3s; animation-delay: 0s; transform: scale(1.33); margin-bottom: 1vh; }
    .FinderIcon { border-radius: 5px ; margin: 0 0 0 4px; height: 5vh; width: 5vh; background-color:red; }
    .AppIcon { border-radius: 5px; margin: 0 0 0 4px; height: 5vh; width: 5vh; background-color:red; }
    .EndIcon { border-radius: 5px; margin: 0 4px; height: 5vh; width: 5vh; background-color:red; }
</style>

<div class="App">
    <img src={currentImage} alt="logo" class="RacWp" />
    <div class="RacScreen">
        <div class="RacMenu">
            <div class="RacLogo">
                <img src={Logo} alt="logo" class="RacLogoImg" />
            </div>
            <div class="RacMenuItems"><div class="MenuFileSct">File</div>
                <div class="MenuEditSct">Edit</div>
                <div class="MenuViewSct">View</div>
                <div class="MenuGoSct">Go</div>
                <div class="MenuWindowSct">Window</div>
                <div class="MenuHelpSct">Help</div>
            </div>
            <div class="RacEmpty">
            </div>
            <div class="RacTime">
                <div class="RacIcons">
                </div>
                <div class="RacTimeText">
                    <div class="RacTimeTextDate">
                        {currentDate}
                    </div>
                    <div class="RacTimeTextHour">
                        {currentTime}
                    </div>
                </div>
                <div class="RacNotify">
                </div>
            </div>
        </div>
        <div class="RacWindow">
            <div class="WindowTest"></div>
        </div>
        <div class="RacDock">
            <div class="FinderIcon"></div>
            <div class="AppIcon"></div>
            <div class="AppIcon"></div>
            <div class="AppIcon"></div>
            <div class="AppIcon"></div>
            <div class="AppIcon"></div>
            <div class="AppIcon"></div>
            <div class="AppIcon"></div>
            <div class="AppIcon"></div>
            <div class="AppIcon"></div>
            <div class="EndIcon"></div>
        </div>
    </div>
</div>