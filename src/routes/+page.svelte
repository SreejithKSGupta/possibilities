<script lang="ts">
let size = 32; // size of the grid (size x size)
let interval = 500; // interval in milliseconds to change the image
$: boxsize = 100 / size; // 100vh divided by the size of each block so that the entire grid fits on the screen
let bitdepth = 3; // bit depth of the color
$:numColors = 2 ** bitdepth;


$: numbers = Array.from({
    length: size
}, (_, i) => i + 1);
$: colors = Array.from({
    length: size * size
}, () => getRandomColor());

let intervalId: number;

function getRandomColor() {
    const r = Math.floor(Math.random() * numColors);
    const g = Math.floor(Math.random() * numColors);
    const b = Math.floor(Math.random() * numColors);
    let rcolor = Math.floor(r * 255 / numColors);
    let gcolor = Math.floor(g * 255 / numColors);
    let bcolor = Math.floor(b * 255 / numColors);
    console.log(r,g,b,rcolor, gcolor, bcolor);
    return `rgb(${rcolor}, ${gcolor}, ${bcolor})`;

}


$: {
    clearInterval(intervalId);
    intervalId = setInterval(() => {
        colors = Array.from({
            length: size * size
        }, () => getRandomColor());
    }, interval);
}
</script>

<div id="appbody">
    <div id="content">
        {#each numbers as row (row)}
        <div class="contentrow">
            {#each numbers as col (col)}
            <div
                style="background-color: {colors[
                (row - 1) * size + (col - 1)
                ]}; width: {boxsize}vh; height: {boxsize}vh;"
                class="box"
                ></div>
            {/each}
        </div>
        {/each}
    </div>
    <div id="controlbar">
        <div class="settingitems">
            <label  class="valuename" for="size">Size</label>
            <input
                class="valueslider"
                type="range"
                min="1"
                max="100"
                step="1"
                title="change the size of the grid (size x size)"
                bind:value={size}
                style="width: 100%;"
                />
            <div class="itemvalue">
                {size} X {size}
            </div>
        </div>
        <div class="settingitems">
            <label class="valuename" for="interval">Interval</label>
            <input
                class="valueslider"
                type="range"
                min="100"
                max="1000"
                step="20"
                title="change the interval in milliseconds to change the image"
                bind:value={interval}
                style="width: 100%;"
                />
            <div class="itemvalue">{interval} ms</div>
        </div>
        <div class="settingitems">
            <label class="valuename" for="bitdepth">Bit Depth</label>
            <input
                class="valueslider"
                type="range"
                min="1"
                max="12"
                title="change the bit depth of the color"
                bind:value={bitdepth}
                style="width: 100%;"
                />
            <div class="itemvalue">{bitdepth} bit, { numColors **3} colours</div>
        </div>
    </div>
</div>

<style>
#appbody {
    display: flex;
    flex-direction: row;
    justify-content: space-evenly;
    align-items: center;
    height: 100vh;
    width: 100vw;
}

#controlbar {
    display: flex;
    flex-direction: column;
    width: 30vw;
}

#content {
    height: 100vh;
    width: 100vh;
    display: flex;
    flex-direction: column;
    flex-wrap: wrap;
    justify-content: center;
}

.contentrow {
    display: flex;
    flex-direction: row;
    width: 100%;
}

.settingitems {
    display: flex;
    flex-direction: row;
    align-items: center;
    width: 100%;
    margin: 10px;
    background-color: rgb(32, 0, 61);
    padding: 10px;
    border-radius: 10px;
    color: white;
    font-weight: bolder;
}

.valuename {
    width: 20%;
}

.valueslider {
    width: 60%;
}

.itemvalue {
    text-align: center;
    margin-top: 10px;
    width: 20%;
}

.valueslider {
    -webkit-appearance: none;
    appearance: none;
    height: 25px;
    background: #033942;
    outline: none;
    opacity: 0.7;
    -webkit-transition: .2s;
    transition: opacity .2s;
    border-radius: 10px;
}

.valueslider:hover {
    opacity: 1;
}

.valueslider::-webkit-slider-thumb {
    -webkit-appearance: none;
    appearance: none;
    width: 25px;
    height: 25px;
    background: #4CAF50;
    cursor: pointer;
    border-radius: 50%;
}

.valueslider::-moz-range-thumb {
    width: 25px;
    height: 25px;
    background: #4CAF50;
    cursor: pointer;
    border-radius: 50%;
}
</style>
