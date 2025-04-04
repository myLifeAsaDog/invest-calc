<script lang="ts">
  import LineChart from './components/LineChart.svelte'

  const TAX: number = 20.315 // 税金
  const TAX_COEFFICIENT = (100 - TAX) / 100 // 税金係数
  const DIGIT: number = 10000 // 万円表示用の係数

  let purchase_price: number = 100 // 購入金額
  let base_price: number = 10000 // 購入時基準価格
  let distribution: number = 100 // 分配金
  let found_fee: number = 0.0938 // 信託手数料
  let interest: number = 5.3 // 年利回り

  $: _purchase_price = purchase_price * DIGIT
  $: _interest = interest / 100 + 1

  $: dependents = Math.ceil((_purchase_price / base_price) * 10000)
  $: found_cost = Math.floor(_purchase_price * (found_fee / 100))
  $: result = Math.floor((dependents * distribution) / 10000)
  $: afterTax = Math.floor(result * TAX_COEFFICIENT)
  $: single_interest = Math.floor(_purchase_price * _interest)
  $: three_interest = Math.floor(_purchase_price * _interest ** 3)
  $: five_interest = Math.floor(_purchase_price * _interest ** 5)
  $: ten_interest = Math.floor(_purchase_price * _interest ** 10)

  $: data = {
    single: single_interest,
    three: three_interest,
    five: five_interest,
    ten: ten_interest
  }
</script>

<main>
  <article>
    <h1>投資信託分配金</h1>
    <!-- svelte-ignore a11y-label-has-associated-control -->
    <dl>
      <dt>購入金額</dt>
      <dd>
        <input type="text" size="6" bind:value={purchase_price} /><span
          >万円</span
        >
      </dd>
      <dt>購入時基準価格</dt>
      <dd>
        <input type="text" size="6" bind:value={base_price} /><span>円</span>
      </dd>
      <dt>信託手数料</dt>
      <dd>
        <input type="text" size="4" bind:value={found_fee} /><span>％</span>
      </dd>
      <dt>分配金(年)</dt>
      <dd>
        <input type="text" size="6" bind:value={distribution} /><span>円</span>
      </dd>
      <dt>年間騰落率</dt>
      <dd>
        <input type="text" size="3" bind:value={interest} /><span>％</span>
      </dd>
    </dl>
  </article>
  <article>
    <dl>
      <dt>約定口数</dt>
      <dd>{dependents.toLocaleString()}&nbsp;<span>口</span></dd>
      <dt>信託手数料</dt>
      <dd>{found_cost.toLocaleString()}&nbsp;<span>円/年</span></dd>
      <dt>受取分配金</dt>
      <dd>{result.toLocaleString()}&nbsp;<span>円</span></dd>
      <dt>税引き後</dt>
      <dd>{afterTax.toLocaleString()}&nbsp;<span>円</span></dd>
      <dt>単年利周り</dt>
      <dd>{single_interest.toLocaleString()}&nbsp;<span>円</span></dd>
      <dt>3年複利</dt>
      <dd>{three_interest.toLocaleString()}&nbsp;<span>円</span></dd>
      <dt>5年複利</dt>
      <dd>{five_interest.toLocaleString()}&nbsp;<span>円</span></dd>
      <dt>10年複利</dt>
      <dd>{ten_interest.toLocaleString()}&nbsp;<span>円</span></dd>
    </dl>
  </article>
</main>
<LineChart {...data} />

<style lang="css">
  * {
    padding: 0;
    margin: 0;
    font-family: sans-serif;
    font-size: 16px;
    font-weight: normal;
    text-decoration: none;
    line-height: 1;
    color: #f1f1f1;
  }
  :global(html, body) {
    background: #333333;
    height: 100%;
  }
  main {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    width: 100%;
  }
  article {
    padding: 20px;
    margin: 20px 0;
    width: 400px;
    border-top: solid 2px #cccccc;
    border-bottom: solid 2px #cccccc;
    box-sizing: border-box;
    overflow: hidden;
  }
  article:first-child {
    border-left: solid 2px #cccccc;
    border-right: solid 1px #ffffff;
  }
  article:last-child {
    border-left: solid 1px #cccccc;
    border-right: solid 2px #cccccc;
  }
  h1 {
    padding: 10px 0;
    margin: 0 0 40px 0;
    line-height: 1.8;
    text-align: center;
  }
  article:last-child dt {
    float: left;
    clear: both;
    line-height: 24px;
  }
  dt,
  dd {
    padding: 10px;
  }
  article:first-child dt {
    border-bottom: solid 1px #cccccc;
  }
  dd {
    font-size: 24px;
    text-align: right;
  }
  dd span {
    font-size: 12px;
  }
  dd input {
    position: relative;
    padding: 0 5px;
    font-size: 24px;
    text-align: right;
    background: transparent;
    border: none;
    border-bottom: solid 2px transparent;
  }
  dd input:focus {
    outline: 0;
    border-bottom: solid 2px #cccccc;
    transition: all 0.5s ease;
  }
</style>
