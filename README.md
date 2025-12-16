<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Winter Elf Name Generator</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Astloch:wght@700&family=Charm:wght@400;700&family=Mountains+of+Christmas&display=swap');

        /* --- Main Layout & Reset --- */
        * {
            box-sizing: border-box;
        }

        html, body {
            margin: 0;
            padding: 0;
            height: 100%;
            width: 100%;
            background: linear-gradient(180deg, #b3e5fc, #e1f5fe);
            background-image: url('https://wallpapers-clan.com/wp-content/uploads/2024/03/starfall-night-sky-mountains-aesthetic-gif-preview-desktop-wallpaper.gif');
            background-size: cover;
            background-position: center;
            background-repeat: no-repeat;
            font-family: Arial, sans-serif;
            overflow-x: hidden; /* Prevent horizontal scroll only */
            overflow-y: auto;   /* Allow vertical scroll if needed */
            position: relative;
        }

        /* --- Snow Animation Layer --- */
        .snow-container {
            position: fixed; /* Fixed so it stays put while scrolling */
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: 0;
        }

        .snowflake {
            position: absolute;
            top: -10px;
            color: white;
            font-size: 1em;
            font-family: Arial, sans-serif;
            text-shadow: 0 0 5px #000;
            animation-name: fall, shake;
            animation-duration: 10s, 3s;
            animation-timing-function: linear, ease-in-out;
            animation-iteration-count: infinite, infinite;
        }

        @keyframes fall {
            0% { top: -10%; }
            100% { top: 110%; }
        }

        @keyframes shake {
            0% { transform: translateX(0px); }
            50% { transform: translateX(80px); }
            100% { transform: translateX(0px); }
        }

        /* --- Content Container --- */
        #content-container {
            position: relative;
            z-index: 10;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center; /* Centers everything vertically */
            min-height: 100vh; /* Use min-height to allow scrolling on small screens */
            padding: 20px;
            text-align: center;
        }

        /* --- Typography --- */
        h1 {
            font-family: "Astloch", serif;
            font-weight: 700;
            margin: 0 0 10px 0;
            /* Use clamp to make it dynamic: min 2.5rem, pref 5vw, max 3.5rem */
            font-size: clamp(2.5rem, 5vw, 3.5rem); 
            color: white;
            text-shadow: 0 0 10px #00BFFF, 0 0 20px #00BFFF;
            line-height: 1;
        }

        h3 {
            font-family: "Charm", cursive;
            font-weight: 700;
            color: white;
            margin: 0 0 15px 0;
            /* Dynamic sizing: min 1.2rem, pref 3vw, max 1.5rem */
            font-size: clamp(1.2rem, 3vw, 1.5rem);
            text-shadow: 
                0 0 8px #ff99cc,
                0 0 12px #ff99cc,
                0 0 18px #ff99cc,
                0 0 8px #99ccff,
                0 0 12px #99ccff,
                0 0 18px #99ccff;
        }

        /* --- Interactive Elements --- */
        button.generate-btn {
            font-family: "Mountains of Christmas", serif;
            /* Dynamic sizing: min 1.2rem, pref 3vw, max 1.5rem */
            font-size: clamp(1.2rem, 3vw, 1.5rem);
            font-weight: bold;
            padding: 8px 25px; /* Slightly reduced padding */
            background-color: #ffb7b2;
            border: 2px solid white;
            border-radius: 50px;
            cursor: pointer;
            box-shadow: 0 4px 15px rgba(255, 183, 178, 0.6);
            transition: all 0.3s ease;
            color: #5c2b2b;
            margin-bottom: 20px;
        }

        button.generate-btn:hover {
            background-color: #ffe4e1;
            transform: scale(1.05);
            box-shadow: 0 6px 20px rgba(255, 255, 255, 0.8);
        }

        button.generate-btn:active {
            transform: scale(0.95);
        }

        /* --- Output Box --- */
        .output-box {
            position: relative;
            padding: 1rem;
            width: 90%;
            max-width: 550px; 
            
            /* Ensures it doesn't touch the bottom or stretch too far */
            max-height: 55vh; /* Limit height to 55% of viewport */
            overflow-y: auto; /* Enable vertical scrolling for the names list */
            min-height: 180px;
            margin-bottom: 20px;

            /* Flex to center text INSIDE the box */
            display: flex;
            flex-direction: column;
            align-items: center;
            
            /* Background settings */
            background-color: rgba(173, 216, 230, 0.85);
            background-image: url('https://i.pinimg.com/originals/02/42/9a/02429ad821287ebbe8f1ab4edf7ce293.gif');
            background-size: cover;
            background-repeat: no-repeat;
            background-position: center;
            
            border-radius: 20px;
            border: 3px solid rgba(255, 255, 255, 0.6);
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
            
            /* Scrollbar styling for Webkit browsers */
            scrollbar-width: thin;
            scrollbar-color: #ffb7b2 rgba(255, 255, 255, 0.5);
        }

        /* Custom scrollbar for Chrome/Safari */
        .output-box::-webkit-scrollbar {
            width: 8px;
        }
        .output-box::-webkit-scrollbar-track {
            background: rgba(255, 255, 255, 0.3);
            border-radius: 10px;
        }
        .output-box::-webkit-scrollbar-thumb {
            background-color: #ffb7b2;
            border-radius: 10px;
        }

        .results-list {
            width: 100%;
            margin-bottom: 30px; /* Space for copy button */
        }

        .elf-entry {
            border-bottom: 1px dashed rgba(255, 255, 255, 0.5);
            padding: 10px 0;
        }
        
        .elf-entry:last-child {
            border-bottom: none;
        }

        .elf-name-text {
            font-family: "Mountains of Christmas", serif;
            /* Dynamic sizing: min 1.5rem, pref 4vw, max 2.2rem */
            font-size: clamp(1.5rem, 4vw, 2.2rem);
            font-weight: bold;
            color: #4b0082;
            text-shadow: 0 0 8px #ffffff, 0 0 12px #ffccff, 0 0 18px #ff99cc;
            margin: 0;
            line-height: 1.1;
        }

        .elf-job-text {
            font-family: "Mountains of Christmas", serif;
            /* Dynamic sizing: min 1.1rem, pref 3vw, max 1.4rem */
            font-size: clamp(1.1rem, 3vw, 1.4rem);
            font-weight: normal;
            color: #2e0052;
            text-shadow: 0 0 5px #ffffff;
            margin-top: 5px;
            font-style: italic;
        }

        /* --- Copy Button (Sticky) --- */
        .copy-btn {
            margin-top: 10px;
            font-size: 0.8rem;
            padding: 6px 15px;
            background: rgba(255, 255, 255, 0.9);
            border: 2px solid #ffb7b2;
            border-radius: 15px;
            cursor: pointer;
            font-family: sans-serif;
            color: #4b0082;
            position: sticky; /* Keeps it visible at bottom of scroll box */
            bottom: 5px;
            box-shadow: 0 -2px 10px rgba(0,0,0,0.1);
            transition: all 0.2s;
        }

        .copy-btn:hover {
            background: #fff;
            transform: scale(1.05);
        }

        /* Mobile adjustments - kept for extra safety, though clamp handles most of it */
        @media (max-width: 600px) {
            .output-box { padding: 1rem; min-height: 160px; }
        }
    </style>
</head>
<body>

    <!-- Snow Animation Container -->
    <div id="snow-container" class="snow-container"></div>

    <div id="content-container">
        <h1>Winter Elf Name</h1>
        
        <button class="generate-btn" onclick="generateName()">Name Me</button>
        
        <h3>Your Magical Winter Elf Names Are...</h3>
        
        <div class="output-box">
            <div id="results-container" class="results-list">
                <div class="elf-entry">
                    <div class="elf-name-text">Click the button!</div>
                </div>
            </div>
            
            <button class="copy-btn" onclick="copyToClipboard()">📋 Copy All</button>
        </div>
    </div>

    <script>
        // --- Data Lists ---
        const firstNames = [
            "Juniper", "Ginger", "Joy", "Crabapple", "Merry", "Cinnamon", "Yule", "Holiday", 
            "Santa", "Cookie", "Ornament", "Nutcracker", "S'mores", "Noel", "Eggnog", "Chestnut", 
            "Pinecone", "Mistletoe", "Evergreen", "Icicle", "Candlelight", "Jolly", "Fruitcake", 
            "Nutmeg", "Icing", "Blizzard", "Apple", "Angel", "Ivy", "Aspen", "Alaska", "North", 
            "Snickers", "Skittles", "Cocoa", "Hickory", "Blaze", "Blessing", "Winter", "Icelyn", 
            "Jack", "Bear", "Bunny", "Solstice", "Aurora", "Hope", "Meatloaf", "Dumpling", "Gem", 
            "Garnet", "Carrot", "Gale", "January", "December", "February", "Ebenezer", "Buddy", 
            "Bilbo", "Challa", "Equinox", "Faith", "Elderberry", "Syrup", "Tangerine", "Luna", 
            "Cardamom", "Chickadee", "Cardinal", "Peppermint", "Melody", "Scarlet", "Mistletoe", 
            "Rudolph", "Dasher", "Vixen", "Walnut", "Saffron", "Cranberry", "Chai", "Moonlight", 
            "Bobbie", "Lars", "Yoyo", "Blitzen", "Snowfall", "Cherry", "Snickers", "Kitty", 
            "Snowdrop", "Poinsettia", "Marshmallow", "Jelly", "Goose", "Grinchy", "Garland", 
            "Revelry", "Carol", "Puck", "Tootsie", "Gumdrop", "Thistle", "Gleam", "Socks", 
            "Fudge", "Birdy", "Filigree", "Chip", "Caribou", "Ermine", "Raven", "Permafrost", 
            "Niveous", "Blanket", "Belle", "Raffia", "Tulle", "Ruby", "Sapphire", "Amethyst", 
            "Bliss", "Opal", "Mouse", "Mulberry", "Muffy", "Goggles", "Toboggan", "Cedar", 
            "Torchy", "Comet", "Sequin", "Tannenbaum", "Spangles", "Marabou", "Bobbin", "Organza", 
            "Tassel", "Umber", "Hermy", "Kristof", "Gizmo", "Treacle", "Molasses", "Scrumptious", 
            "Caramel", "Butterscotch", "Amaryllis", "Clove", "Hemlock", "Pear", "Ferny", "Pixie", 
            "Serendipity", "Reverie", "Rolly"
        ];

        const sirPrefixes = [
            "Sparkle", "Frost", "Chilly", "Sprinkle", "Pine", "Snow", "Jingle", "Sugar", 
            "Frosting", "Orange", "Crystal", "Icey", "Holly", "Festive", "Elf", "Elven", 
            "Chocolate", "Menorah", "Candle", "Music", "Jolly", "Button", "Stocking", "Ribbon", 
            "Flurry", "Red", "Green", "Gold", "Silver", "Birch", "Sleigh", "Bright", "Glow", 
            "Twinkle", "Candy", "Crystal", "River", "Hockey", "Ivy", "Coal", "Tinsel", "Bitter", 
            "Nippy", "Brandy", "Brisk", "Cider", "Foggy", "Misty", "Scrooge", "Sniffle", "Rosy", 
            "Sleepy", "Cinder", "Dreidel", "Moon", "Glitter", "Pumpkin", "Toffee", "Cozy", 
            "Breezy", "Noodle", "Toasted", "Roasted", "Dazzle", "Chimney", "Ever", "Pudding", 
            "Glacier", "Ember", "Citrus", "Penguin", "Giggle", "Bauble", "Tundra", "Polar", 
            "Woolly", "White", "Shimmer", "Velvet", "Diamond", "Honey", "Crumble", "Fragile", 
            "Satin", "Lantern", "Waxy", "Night", "Slumber", "Maple", "Cotton", "Downy", "Dimple", 
            "Kringle", "Sweet", "Whimsy", "Wonder", "Gentle", "Fizzy", "Wren", "Sparrow"
        ];

        const sirSuffixes = [
            "fir", "butter", "wind", "pie", "bottom", "glimmer", "bell", "branch", "flake", 
            "berry", "bark", "tree", "hearth", "present", "doll", "wreath", "feast", "sleigh", 
            "skate", "heart", "ski", "sled", "milk", "feast", "star", "light", "mint", "gift", 
            "cover", "chime", "swag", "hearth", "flame", "fall", "muffin", "cane", "foot", 
            "wool", "bow", "wish", "mitten", "boot", "crisp", "melt", "wine", "soup", "dove", 
            "berg", "blossom", "bread", "harp", "plum", "wing", "leaf", "tea", "song", "cheeks", 
            "nose", "spice", "shine", "gloss", "peak", "twig", "puff", "fluff", "gust", "toes", 
            "fig", "glitz", "clock", "mas", "bough", "hymn", "bun", "rime", "drift", "sleet", 
            "hallow", "scarf", "glove", "melt", "veil", "mist", "twine", "fox", "tide", "slush", 
            "banner", "sash", "wisp", "brittle", "pot", "glaze", "wick", "log", "wood", "tart", 
            "nut"
        ];
        
        const jobs = [
            "Chief Toy Maker", "Reindeer Flight Instructor", "Hot Cocoa Taster", "Snowflake Architect", 
            "Chimney Sweep Specialist", "Sleigh Mechanic", "Naughty List Auditor", "Nice List Manager", 
            "Gift Wrapping Expert", "Ribbon Curler", "Tinsel Tangler", "Ornament Hanger", 
            "Gingerbread Decorator", "Snowball Fight Coach", "Glitter Technician", "Pine Needle Sweeper", 
            "Icicle Polisher", "Candy Cane Twister", "Stocking Stuffer", "Mistletoe Coordinator", 
            "Star Duster", "Bell Ringer", "Caroling Director", "Cookie Quality Control", "Toy Tester", 
            "Gumdrop Sorter", "Map Reader", "Sleigh GPS Navigator", "Stable Hand", "Carrot Peeler", 
            "Coal Miner (for the naughty kids)", "Snow Angel Artist", "Fudge Stirrer", "Yule Log Burner", 
            "Party Planner", "Elf Shoe Cobbler", "Hat Pom-Pom Fluffer"
        ];

        function getRandomItem(array) {
            return array[Math.floor(Math.random() * array.length)];
        }

        // Store current names for copying
        let currentGeneratedNames = [];

        function generateName() {
            const container = document.getElementById('results-container');
            container.innerHTML = ''; // Clear previous results
            currentGeneratedNames = []; // Reset copy buffer

            for (let i = 0; i < 5; i++) {
                const first = getRandomItem(firstNames);
                const prefix = getRandomItem(sirPrefixes);
                const suffix = getRandomItem(sirSuffixes);
                const job = getRandomItem(jobs);
                
                const fullName = `${first} ${prefix}${suffix}`;
                
                // Add to copy buffer
                currentGeneratedNames.push(`${fullName}, ${job}`);

                // Create HTML elements
                const entryDiv = document.createElement('div');
                entryDiv.className = 'elf-entry';
                
                const nameDiv = document.createElement('div');
                nameDiv.className = 'elf-name-text';
                nameDiv.innerText = fullName;
                
                const jobDiv = document.createElement('div');
                jobDiv.className = 'elf-job-text';
                jobDiv.innerText = job;
                
                entryDiv.appendChild(nameDiv);
                entryDiv.appendChild(jobDiv);
                container.appendChild(entryDiv);
            }

            // Add pop animation
            const outputBox = document.querySelector('.output-box');
            outputBox.style.transform = "scale(1.02)";
            setTimeout(() => {
                outputBox.style.transform = "scale(1)";
            }, 200);
            
            // Scroll to top of box so they see the first name
            outputBox.scrollTop = 0;
        }

        function copyToClipboard() {
            if (currentGeneratedNames.length === 0) return;
            
            const fullText = currentGeneratedNames.join('\n');
            
            navigator.clipboard.writeText(fullText).then(() => {
                const btn = document.querySelector('.copy-btn');
                const originalText = btn.innerText;
                btn.innerText = "✨ Copied All! ✨";
                setTimeout(() => {
                    btn.innerText = originalText;
                }, 2000);
            }).catch(err => {
                console.error('Failed to copy: ', err);
            });
        }

        // Initialize Snowflakes
        function createSnowflakes() {
            const snowContainer = document.getElementById('snow-container');
            const numberOfSnowflakes = 30; 
            const snowflakeChars = ['❄', '❅', '❆', '•'];

            for (let i = 0; i < numberOfSnowflakes; i++) {
                const flake = document.createElement('div');
                flake.classList.add('snowflake');
                flake.innerText = snowflakeChars[Math.floor(Math.random() * snowflakeChars.length)];
                
                flake.style.left = Math.random() * 100 + 'vw';
                flake.style.animationDuration = (Math.random() * 5 + 5) + 's, ' + (Math.random() * 3 + 2) + 's';
                flake.style.opacity = Math.random();
                flake.style.fontSize = (Math.random() * 10 + 10) + 'px';
                
                snowContainer.appendChild(flake);
            }
        }

        window.onload = function() {
            createSnowflakes();
            // Scroll to the main content to hide any header text/padding added by hosting
            const container = document.getElementById('content-container');
            if (container) {
                container.scrollIntoView({ behavior: 'smooth', block: 'start' });
            }
        };

    </script>
</body>
</html>
