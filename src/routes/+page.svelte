<script>
    import { onMount } from "svelte";
    import stupidFaust from "../img/stupidfaust.png";
    import furinaNerd from "../img/furinanerd.jpg";

    const days = [
        "Minggu",
        "Senin",
        "Selasa",
        "Rabu",
        "Kamis",
        "Jumat",
        "Sabtu"
    ];

    const schedule = [
        {
            time: "07.30 - 08.20",
            start: "07:30",
            end: "08:20",
            days: ["-", "Matematika Diskrit", "Org. & Arsi. Komputer", "Pancasila", "-"]
        },
        {
            time: "08.20 - 09.10",
            start: "08:20",
            end: "09:10",
            days: ["-", "Matematika Diskrit", "Org. & Arsi. Komputer", "Pancasila", "-"]
        },
        {
            time: "09.10 - 10.00",
            start: "09:10",
            end: "10:00",
            days: ["-", "Matematika Diskrit", "Org. & Arsi. Komputer", "Pancasila", "-"]
        },
        {
            time: "10.00 - 10.50",
            start: "10:00",
            end: "10:50",
            days: ["-", "Matematika Diskrit", "Org. & Arsi. Komputer", "Pancasila", "-"]
        },
        {
            time: "10.50 - 11.40",
            start: "10:50",
            end: "11:40",
            days: ["Algo & Pemrograman", "Matematika Diskrit", "Sistem Operasi", "Pendidikan Agama", "-"]
        },
        {
            time: "11.40 - 12.30",
            start: "11:40",
            end: "12:30",
            days: ["Lunch + Dzuhur", "Lunch + Dzuhur", "Lunch + Dzuhur", "Lunch + Dzuhur", "-"]
        },
        {
            time: "12.30 - 13.20",
            start: "12:30",
            end: "13:20",
            days: ["Algo & Pemrograman", "Matematika Diskrit", "Sistem Operasi", "Pendidikan Agama", "-"]
        },
        {
            time: "13.20 - 14.10",
            start: "13:20",
            end: "14:10",
            days: ["Algo & Pemrograman", "English for TIK", "Sistem Operasi", "Pendidikan Agama", "-"]
        },
        {
            time: "14.10 - 15.00",
            start: "14:10",
            end: "15:00",
            days: ["Algo & Pemrograman", "English for TIK", "Sistem Operasi", "Pendidikan Agama", "-"]
        },
        {
            time: "15.00 - 15.15",
            start: "15:00",
            end: "15:15",
            days: ["Sholat Ashar", "Sholat Ashar", "Sholat Ashar", "-", "-"]
        },
        {
            time: "15.15 - 16.05",
            start: "15:15",
            end: "16:05",
            days: ["Algo & Pemrograman", "English for TIK", "Sistem Operasi", "-", "-"]
        },
        {
            time: "16.05 - 16.55",
            start: "16:05",
            end: "16:55",
            days: ["Algo & Pemrograman", "English for TIK", "Sistem Operasi", "-", "-"]
        }
    ];

    let currentMinutes = $state(0);
    let currentDayIndex = $state(-1); // 0 = Senin, 1 = Selasa, ..., 4 = Jumat
    let dateStr = $state("");
    let dayName = $state("");

    function timeToMinutes(timeStr) {
        const [h, m] = timeStr.split(".").join(":").split(":").map(Number);
        return h * 60 + m;
    }

    function updateDateTime() {
        const now = new Date();
        const day = String(now.getDate()).padStart(2, "0");
        const month = String(now.getMonth() + 1).padStart(2, "0");
        dateStr = `${day} / ${month}`;
        dayName = days[now.getDay()];

        // getDay(): 0 = Minggu, 1 = Senin, ..., 5 = Jumat, 6 = Sabtu
        currentDayIndex = now.getDay() - 1;
        currentMinutes = now.getHours() * 60 + now.getMinutes();
    }

    updateDateTime();

    onMount(() => {
        updateDateTime();
        const interval = setInterval(updateDateTime, 30000);
        return () => clearInterval(interval);
    });

    function isSlotActive(start, end, dayIdx, subject) {
        if (subject === "-") return false;
        if (dayIdx !== currentDayIndex) return false;
        const startMin = timeToMinutes(start);
        const endMin = timeToMinutes(end);
        return currentMinutes >= startMin && currentMinutes < endMin;
    }
</script>

<div class="background">
    <h1 class="class-title">Jadwal Kuliah Prodi Teknik Informatika Kelas 1A</h1>
    <div class="class-items">
        <div class="board">
            <div class="board-main">
                <div class="board-content">
                    <div class="top-board">
                        <div class="time">
                            <h1>Time</h1>
                        </div>
                        <div class="days">
                            <h1 class="days-entry">Monday</h1>
                            <h1 class="days-entry">Tuesday</h1>
                            <h1 class="days-entry">Wednesday</h1>
                            <h1 class="days-entry">Thursday</h1>
                            <h1 class="days-entry">Friday</h1>
                        </div>
                    </div>
                    <hr>
                    <div class="schedule-board">
                        {#each schedule as item, i}
                            <div class="schedule-entry">
                                <div class="time">
                                    <h2>{item.time}</h2>
                                </div>
                                <div class="days">
                                    {#each item.days as subject, dayIdx}
                                        <h3
                                            class="days-entry"
                                            class:active-matkul={isSlotActive(item.start, item.end, dayIdx, subject)}
                                        >
                                            {subject}
                                        </h3>
                                    {/each}
                                </div>
                            </div>
                            {#if i < schedule.length - 1}
                                <hr>
                            {/if}
                        {/each}
                    </div>
                </div>
            </div>
        </div>
        <div class="side">
            <div class="date">
                <div class="day-month">
                    <h1>
                        {dateStr}
                    </h1>
                    <h1>
                        {dayName}
                    </h1>
                </div>
            </div>
            <div class="sidenotes">
            <div class="keterangan-mata-kuliah">
                <h2>Keterangan MatKul             </h2>
                <h3>Matkul | Dosen | PJ | Ruangan </h3>
                <h4>AP | Pak Ariawan | Chaeza  | AA301          </h4>
                <hr>          
                <h4>MTD | Bu Euis | Talitha     | GSG202          </h4>
                <hr>           
                <h4>ENG | Pak Denil | Azzam   | GSG202          </h4>
                <hr>           
                <h4>OAK | Pak Asep | Yizreel    | GSG209         </h4>
                <hr>           
                <h4>OS | Pak Anggi | Ojan | GSG202          </h4>
                <hr>           
                <h4>PP | Bu Ayres | Abid Rahmat | Perpus 5          </h4>
                <hr>          
                <h4>PAI | Bu iin | Rainier     | Perpus 4          </h4>
                <h2>
                    Keterangan Tambahan
                </h2>
                <h3>
                    Ketua Kelas | Raymond
                </h3>
                <h3>Wakil Ketua  | Azizi </h3>
                
            </div>
        </div>
        </div>
        
        
    </div>
    <h2>
        Author: Fauzan Rizky Naufal | Last Updated: 19 November 2026
    </h2>
</div>
<div class="mobile">
    <img id="mobile-warning" src={furinaNerd} alt="">
    <p class="mobile-warning-text">Maff yh &gt;~&lt; buat sekarang website ini khusus Desktop🙏</p>
    <p class="mobile-warning-text">Hehehehhehehe</p>
</div>

<style>
    * {
        font-family: "Kalam", cursive;
        /* outline: 1px solid red; */
    }

    .class-title {
        height: 7%;
        text-align: center;
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .background {
        width: 100vw;
        height: 100vh;
        padding: 1rem;
        background-color: #2a42e4;
        color: white;
    }

    .class-items {
        display: flex;
        height: 90%;
    }

    .board {
        width: 80%;
        height: fit-content;
        padding: 1.5rem;
        color: white;
    }

    hr {
        color: white;
    }

    .board-main {
        width: 100%;
        height: 100%;

        background-color: #7dbd7b;
        outline: 5px solid #b78043;
        border-bottom: 10px solid #b78043;

        padding: 1rem;
    }

    .board-content {
        width: 100%;
        height: 100%;

        padding: 0.5rem;
        border: 2px solid white;
        border-radius: 15px;
    }

    .top-board {
        display: flex;
    }

    .schedule-board {
        display: flex;
        flex-direction: column;
    }

    .schedule-entry {
        display: flex;
    }

    .time {
        display: flex;
        justify-content: center;
        width: 15%;
    }

    .days {
        display: flex;
        width: 85%;
    }

    .days-entry {
        border-left: 1px solid white;
        width: 20%;
        display: flex;
        align-items: center;
        justify-content: center;
        text-align: center;
        padding: 0.15rem 0.25rem;
        transition: all 0.3s ease;
    }

    .days-entry.active-matkul {
        background-color: #dc2626;
        color: #ffffff;
        font-weight: 700;
        border-radius: 6px;
        box-shadow: 0 0 10px rgba(239, 68, 68, 0.9);
        animation: pulse-red 1.6s infinite ease-in-out;
    }

    @keyframes pulse-red {
        0%, 100% {
            box-shadow: 0 0 6px rgba(239, 68, 68, 0.6);
            transform: scale(1);
        }
        50% {
            box-shadow: 0 0 14px rgba(255, 90, 90, 1);
            transform: scale(1.02);
        }
    }

    .side {
        display: flex;
        flex-direction: column;
        height: 100%;
    }

    .date {
        width: 100%;
        height: 30%;
        display: flex;
        flex-direction: column;
        align-items: flex-end;
        justify-content: center;
        padding: 1rem;
    }

    .day-month {
        background-color: white;
        color: black;
        padding: 0.25rem 1.5rem;
        border-top: 10px solid #b78043;
    }

    .sidenotes {
        height: 100%;
        width: 100%;
        padding: 1rem;
    }

    .keterangan-mata-kuliah {
        width: 100%;
        height: 70%;
        background-color: white;
        color: black;
        padding: 0.5rem;
        text-align: justify;
    }

    p {
        text-align: center;
    }

    .mobile {
        display: none;
    }

    h2 {
        text-align: center;
    }

    @media only screen and (max-width: 700px) {
        .background {
            display: none;
        }

        #mobile-warning {
            width: 50%;
        }

        .mobile-warning-text {
            width: 75%;
        }

        .mobile {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            width: 100vw;
            height: 100vh;
            background-color: white;
            color: black;

            gap: 50px;
            font-weight: 900;
        }
    }
</style>