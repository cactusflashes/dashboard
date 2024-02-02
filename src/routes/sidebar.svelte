<script>
    import { onMount } from "svelte";
    const apiKey = import.meta.env.VITE_API_KEY;

    //time and formatting
    var currentDate = new Date();
    var hours = currentDate.getHours();
    var minutes = currentDate.getMinutes();
    var amPm = hours >= 12 ? "PM" : "AM";
    hours = hours % 12 || 12;
    var formattedHours = hours < 10 ? hours : hours;
    var formattedMinutes = minutes < 10 ? "0" + minutes : minutes;
    var formattedTime = formattedHours + ":" + formattedMinutes + " " + amPm;

    //day of the week
    var currentDate = new Date();
    var dayNames = [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
    ];
    var currentDayString = dayNames[currentDate.getDay()];

    //dynamic greeting
    function getGreeting() {
        let currentHour = new Date().getHours();

        if (currentHour >= 5 && currentHour < 12) {
            return "morning";
        } else if (currentHour >= 12 && currentHour < 18) {
            return "afternoon";
        } else if (currentHour >= 18 && currentHour < 21) {
            return "evening";
        }
    }

    let greeting = getGreeting();

    //weather
    const url = `https://api.openweathermap.org/data/2.5/weather?lat=35.1987522&lon=-111.6518229&appid=${apiKey}&units=imperial`;
    let currentTemp;
    let currentConditions;

    fetch(url)
        .then((response) => response.json())
        .then((data) => {
            currentTemp = data.main.temp.toFixed(0);
            currentConditions = data.weather[0].description;
        })
        .catch((error) => {
            console.error("Error:", error);
        });

    //refresh page every 60 seconds.
    onMount(() => {
        setInterval(() => {
            window.location.href = window.location.href;
        }, 60000);
    });
</script>

<div class="sidebar">
    <div class="sidebar-text">
        <h2>{formattedTime}</h2>
        <h3 style="margin: 0; opacity: 0.6;">{currentDayString}</h3>
        <h2>Good {greeting}, Joseph.</h2>
        <h3>
            It's currently {currentTemp}º with {currentConditions} this {greeting}.
        </h3>
    </div>
</div>

<style>
    .sidebar {
        display: flex;
        flex-direction: column;
        position: fixed;
        height: 80%;
        width: 250px;
        top: 10%;
        left: 5%;
        border-radius: 20px;
        -webkit-backdrop-filter: blur(10px);
        backdrop-filter: blur(10px);
        background-color: rgba(255, 255, 255, 0.2);
        border: 1px solid rgba(255, 255, 255, 0.3);
        align-items: center;
        justify-content: center;
    }

    .sidebar-text {
        display: flex;
        flex-direction: column;
        height: 95%;
        width: 80%;
    }

    @media all and (max-width: 700px) {
        .sidebar {
            left: 15%;
        }
    }
</style>
