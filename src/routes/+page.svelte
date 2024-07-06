<script lang="ts">
    import { onMount } from "svelte";
    function addToEquation(value: string) {
        equation += value;
    }

    function backspace() {
        switch (equation.substring(equation.length - 3, equation.length)) {
            case " + ":
            case " * ":
            case " - ":
            case " / ":
                equation = equation.substring(0, equation.length - 3);
                break;
            default:
                equation = equation.substring(0, equation.length - 1);
        }
    }

    function Clear() {
        equation = "";
    }

    function solve() {
        try {
           let answer = eval(equation);
           if (answer == undefined) throw SyntaxError;
           equation = answer;
        } catch (error) {
            let output = document.getElementById("output");
            output?.classList.add("bg-red-500");
            setTimeout(() => {
            output?.classList.remove("bg-red-500");
            }, 650);
        }
    }

    function calculateSquareRoot() {
    try {
      const result = Math.sqrt(eval(equation));
      equation = result.toString();
    } catch (error) {
    }
}

    let equation: string="";

    function onKeyDown(e: KeyboardEvent) {
        let button = document.getElementById(e.key);
        button?.click();
        button?.focus();
        setTimeout(() => {
            // @ts-ignore
            document.activeElement?.blur();
        }, 100);
    }
    
    onMount(() => {
        let allButtons = document.getElementsByTagName('button');
        for (let i = 0; i < allButtons.length; i++) {
            allButtons[i].addEventListener('click', () => {
                new Audio('/click.mp3').play();
            });
        }
    });
</script>

<svelte:head>
    <title>
        آلة حاسبة
    </title>
</svelte:head>

<svelte:window on:keydown|preventDefault={onKeyDown} />

     <div class="bg-white h-[30rem] w-[20rem] rounded-3xl grid grid-cols-4 gap 
        p-8 font-bold text-2xl shadow-2xl">
    <div
    id="output"
    class="bg-[#2386fe] rounded-full col-span-4 h-14 flex items-center 
    px-4 mb-3 text-white text-xl overflow-x-auto break-all transition-all">
     {equation}
    </div>

    <!--Row 1-->

    <button id="%" on:click={() => addToEquation(" / 100")} class="bg-[#f3f6fb] text-[#717785] hover:bg-[#f3f6fb]/40 active:bg-[#f3f6fb]">
        %
    </button>
     <!--Square Root Icon-->
    <button on:click={() => calculateSquareRoot()} class="bg-[#f3f6fb] text-[#717785] hover:bg-[#f3f6fb]/40 active:bg-[#f3f6fb] flex items-center justify-center">
        <svg class="w-7 h-7" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24">
            <path fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
             d="M3 12h2l4 8l4-16h8"/></svg>
    </button>
    <button id="Backspace" on:click={() => backspace()} class="bg-[#f3f6fb] text-[#717785] hover:bg-[#f3f6fb]/40 active:bg-[#f3f6fb]">CE</button>
    <button id="Delete" on:click={Clear} class="bg-[#1f2b55] text-white hover:bg-[#1f2b55]/80 active:bg-[#1f2b55]">C</button>
    <!--Row 2-->

    <button id="7" on:click={() => addToEquation('7')}>7</button>
    <button id="8" on:click={() => addToEquation('8')}>8</button>
    <button id="9" on:click={() => addToEquation('9')}>9</button>
    <button id="-" on:click={() => addToEquation(' - ')} class="bg-[#fe4258] text-white hover:bg-[#fe4258]/80 active:bg-[#fe4258]">−</button>
    <!--Row 3-->

    <button id="4" on:click={() => addToEquation('4')}> 4 </button>
    <button id="5" on:click={() => addToEquation('5')}> 5 </button>
    <button id="6" on:click={() => addToEquation('6')}>6</button>
    <button id="/" on:click={() => addToEquation(' / ')} class="bg-[#2088fe] text-white hover:bg-[#2088fe]/80 active:bg-[#2088fe]">÷</button>
    <!--Row 4-->

    <button id="1" on:click={() => addToEquation('1')}>1</button>
    <button id="2" on:click={() => addToEquation('2')}>2</button>
    <button id="3" on:click={() => addToEquation('3')}>3</button>
    <button id="*" on:click={() => addToEquation(' * ')} class="bg-[#fec206] text-white hover:bg-[#fec206]/80 active:bg-[#fec206]">×</button>
    <!--Row 5-->

    <button id="." on:click={() => addToEquation('.')}>.</button>
    <button id="0" on:click={() => addToEquation('0')}>0</button>
    <button id="=" on:click={() => solve()} class="bg-[#f3f6fb] text-[#717785] hover:bg-[#f3f6fb]/40 active:bg-[#f3f6fb]">=</button>
    <button id="+" on:click={() => addToEquation(' + ')} class="bg-[#64dc75] text-white hover:bg-[#64dc75]/80 active:bg-[#64dc75]">+</button>
</div>