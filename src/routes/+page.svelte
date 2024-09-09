<script>
  import * as bip39 from 'bip39';
  import CryptoJS from 'crypto-js';
  import { Buffer } from 'buffer';

  // グローバルにBufferを定義
  if (typeof window !== 'undefined') {
    // @ts-ignore
    window.Buffer = Buffer;
  }

  let providedCode = '';
  let passwordString = '';
  let seedPhrase = '';

  function generateSeedPhrase() {
    try {
      // プロバイドコードとパスワードを組み合わせてハッシュを生成
      const combinedInput = providedCode + passwordString;
      const hash = CryptoJS.SHA256(combinedInput);

      // WordArrayを16進数文字列に変換
      const entropy = hash.toString(CryptoJS.enc.Hex).slice(0, 32);
      console.log(entropy);
      // エントロピーからシードフレーズを生成
      seedPhrase = bip39.entropyToMnemonic(entropy);

      console.log('生成されたシードフレーズ:', seedPhrase);
    } catch (error) {
      console.error('シードフレーズの生成中にエラーが発生しました:', error);
      seedPhrase = 'エラーが発生しました。もう一度お試しください。';
    }
  }
</script>

<main>
  <div class="app-container">
    <h1>Block hand <span>Sheedphrase generator -sample-</span></h1>

    <div class="input-container">
      <input bind:value={providedCode} placeholder="Enter provided ring code" />
      <input bind:value={passwordString} type="password" placeholder="Enter your own password" />
      <button on:click={generateSeedPhrase}>Generate Seed Phrase</button>
    </div>

    {#if seedPhrase}
      <p class="result">Your seed phrase is: {seedPhrase}</p>
    {/if}
  </div>
</main>

<style>
  :root {
    --bg-color: #e0e5ec;
    --shadow-light: #ffffff;
    --shadow-dark: #a3b1c6;
    --text-color: #4a5568;
    --accent-color: #2D8B61;  /* グリーンのアクセントカラー */
  }

  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }


  .app-container {
    max-width: 800px;
    margin: 0 auto;
    padding: 20px;
  }

  h1 {
    font-size: 24px;
    margin-bottom: 20px;
    color: var(--accent-color);
  }

  h1 span {
    font-weight: normal;
    font-size: 18px;
    color: var(--text-color);
  }



  .input-container {
    margin-bottom: 20px;
  }

  input, button {
    margin: 20px;
    width: 60%;
    padding: 12px;
    margin-bottom: 10px;
    border: none;
    border-radius: 5px;
    background-color: var(--bg-color);
    color: var(--text-color);
    box-shadow: inset 2px 2px 5px var(--shadow-dark),
                inset -2px -2px 5px var(--shadow-light);
  }

  button {
    cursor: pointer;
    font-weight: bold;
    transition: all 0.3s ease;
    background-color: var(--accent-color);
    color: white;
    margin-top: 40px;
    width: 40%;
  }

  button:hover {
    box-shadow: 2px 2px 5px var(--shadow-dark),
                -2px -2px 5px var(--shadow-light);
    opacity: 0.9;
  }

  .result {
    margin-top: 20px;
    padding: 10px;
    background-color: var(--bg-color);
    border-radius: 5px;
    box-shadow: inset 2px 2px 5px var(--shadow-dark),
                inset -2px -2px 5px var(--shadow-light);
    color: var(--accent-color);
  }


  input::placeholder {
    color: var(--accent-color);
    opacity: 0.7;
  }
</style>