<script>
    let message = " I LOVE YOU "; // Add space for smoother scrolling
    let grid = [];
    let rows = 9;
    let cols = 30; // Width of the LED display
    let scrollPosition = 0;

    // Characters represented as LED patterns
    const letters = {
        " ": ["000", "000", "000", "000", "000"],
        A: ["010", "111", "101", "101", "111"],
        B: ["110", "101", "110", "101", "110"],
        C: ["011", "100", "100", "100", "011"],
        D: ["110", "101", "101", "101", "110"],
        E: ["111", "100", "111", "100", "111"],
        F: ["111", "100", "111", "100", "100"],
        G: ["011", "100", "101", "101", "011"],
        H: ["101", "111", "101", "101", "101"],
        I: ["111", "010", "010", "010", "111"],
        J: ["001", "001", "001", "101", "111"],
        K: ["101", "110", "101", "110", "101"],
        L: ["100", "100", "100", "100", "111"],
        M: ["101", "111", "111", "101", "101"],
        N: ["101", "111", "111", "101", "101"],
        O: ["011", "101", "101", "101", "011"],
        P: ["111", "101", "111", "100", "100"],
        Q: ["011", "101", "101", "111", "011"],
        R: ["111", "101", "111", "110", "101"],
        S: ["011", "100", "011", "001", "111"],
        T: ["111", "010", "010", "010", "010"],
        U: ["101", "101", "101", "101", "111"],
        V: ["101", "101", "101", "010", "010"],
        W: ["101", "101", "111", "111", "101"],
        X: ["101", "101", "010", "101", "101"],
        Y: ["101", "101", "010", "010", "010"],
        Z: ["111", "001", "011", "100", "111"]
    };

    // Initialize the LED grid
    for (let i = 0; i < rows; i++) {
        grid[i] = Array(cols).fill(false);
    }

    // Draw a character in the grid
    function drawCharacter(char, row, col) {
        const pattern = letters[char] || [];
        for (let r = 0; r < pattern.length; r++) {
            for (let c = 0; c < pattern[r].length; c++) {
                if (col + c >= 0 && col + c < cols && pattern[r][c] === "1") {
                    grid[row + r][col + c] = true;
                }
            }
        }
    }

    // Scroll the message by updating the grid
    function updateGrid() {
        // Clear the grid
        for (let i = 0; i < rows; i++) {
            grid[i].fill(false);
        }

        // Draw the message
        let offset = scrollPosition;
        for (let i = 0; i < message.length; i++) {
            drawCharacter(message[i], 2, offset + i * 3); // Adjust spacing (3px per character)
        }

        // Update scroll position
        scrollPosition--;
        if (scrollPosition + message.length * 3 < 0) {
            scrollPosition = cols; // Reset position for continuous scrolling
        }
    }

    // Start the animation
    setInterval(updateGrid, 500); // Adjust speed here
</script>

<style>
    .led-container {
        display: grid;
        grid-template-rows: repeat(9, 20px);
        grid-template-columns: repeat(30, 20px);
        gap: 5px;
        justify-content: center;
        background-color: black;
        padding: 20px;
        margin: 0 auto;
        overflow: hidden; /* To prevent scrolling bar */
    }
    .led {
        width: 18px;
        height: 18px;
        background-color: #333;
        border-radius: 50%;
        box-shadow: 0 0 4px rgba(255, 0, 0, 0.5);
        transition: background-color 0.3s, box-shadow 0.3s;
    }
    .led.on {
        background-color: red;
        box-shadow: 0 0 8px rgba(255, 0, 0, 0.9);
    }
</style>

<div class="led-container">
    {#each grid as row}
        {#each row as cell}
            <div class="led {cell ? 'on' : ''}"></div>
        {/each}
    {/each}
</div>
