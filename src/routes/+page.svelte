<script lang="ts">
  let pRef: HTMLParagraphElement
  let text = ''

  let conversionDict = {
    'a': '1072',
    'b': '98',
    'c': '1089',
    'd': '100',
    'e': '1077',
    'f': '102',
    'g': '609',
    'h': '104',
    'i': '1110',
    'j': '74',
    'k': '107',
    'l': '108',
    'm': '109',
    'n': '110',
    'o': '1086',
    'p': '1088',
    'q': '113',
    'r': '114',
    't': '116',
    'u': '965',
    'v': '118',
    'w': '969',
    'x': '1093',
    'y': '1091',
    'z': '122',
    '?': '63',
  }

  const placeholder =
    "Amidst the bustling city, where the neon lights flicker and the sounds of traffic echo, there lies a hidden tranquility. In the heart of chaos, a moment of serenity can be found. It's in these quiet moments that the soul finds its peace, and the mind, its clarity. Each day brings new opportunities and challenges, weaving the tapestry of life with threads of laughter, tears, and endless possibilities. Embrace the unknown, for within its depths, the greatest adventures await."

  function convertToLookalike(text: string, dict: { [key: string]: string } = conversionDict) {
    let result = ''
    for (const c of text) {
      const nC = dict[c]
      result += c === '\n' ? '&NewLine;' : nC ? `&#${nC};` : c
    }
    return result
  }

  async function copyParsedText(this: HTMLButtonElement) {
    if (pRef && pRef.textContent) navigator.clipboard.writeText(pRef.textContent)
    this.textContent = 'Copied!'
    this.classList.add('text-green-500')
  }

  function resetBtn(this: HTMLButtonElement) {
    this.textContent = 'Copy'
    this.classList.remove('text-green-500')
  }

  $: {
    const p = pRef
    if (p) p.innerHTML = convertToLookalike(text === '' ? placeholder : text)
  }
</script>

<div
  class="text-white min-h-screen bg-gray-700 flex justify-center items-start py-16 px-5 relative"
>
  <div class="md:w-1/2 space-y-5">
    <div class="w-full flex justify-center items-center">
      <h1 class="font-bold text-3xl font-mono text-center">Unicode Lookalike Parser</h1>
    </div>
    <textarea
      bind:value={text}
      class="text-gray-500 resize rounded w-full h-56 p-10"
      {placeholder}
    />
    <div class="relative w-full flex flex-col md:flex-row justify-center items-center">
      <h1 class="font-bold text-3xl font-mono text-center">Parsed Result</h1>
      <button
        class="md:absolute right-0 py-4 px-6 bg-blue-700 rounded hover:bg-blue-500 transition focus:ring-2"
        on:click={copyParsedText}
        on:blur={resetBtn}
      >
        Copy
      </button>
    </div>
    <p
      bind:this={pRef}
      class="py-5 px-10 whitespace-pre-line break-words ring-4 min-h-[20em] rounded bg-gray-400"
    />
  </div>

  <p class="font-bold text-sm font-mono absolute right-4 top-4">Created by Truong Nguyen</p>
</div>
