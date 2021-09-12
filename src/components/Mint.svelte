<script>
  export let active;

  const onCancel = () => {
    active = false;
  }

  const onSubmit = async () => {
    const psbt = new Psbt();
    psbt
      .addInput(inputData)
      .addIssuance({
        assetAddress: address.fromOutputScript(
          assetPay.payment.output,
          regtest,
        ),
        tokenAddress: address.fromOutputScript(
          tokenPay.payment.output,
          regtest,
        ),
        assetAmount: 100,
        tokenAmount: 1,
        precision: 8,
        confidential: true, // must be true, we'll blind the issuance!
        contract: {
          name: 'testcoin',
          ticker: 'T-COIN',
          entity: {
            domain: 'vulpemventures.com',
          },
          version: 0,
          precision: 8,
        },
      })
      .addOutputs([
        {
          nonce,
          asset,
          value: confidential.satoshiToConfidentialValue(99999500),
          script: alice1.payment.output,
        },
        {
          nonce,
          asset,
          value: confidential.satoshiToConfidentialValue(500),
          script: Buffer.alloc(0),
        },
      ]);

  

    active = false;
  }

</script>

<div class="modal is-active" >
  <div class="modal-background"></div>
  <div class="modal-card">
    <header class="modal-card-head">
      <p class="modal-card-title">Create your NFT</p>
      <button class="delete" aria-label="close" on:click={onCancel}></button>
    </header>
    <section class="modal-card-body">
      <!-- Content ... -->
    </section>
    <footer class="modal-card-foot">
      <button class="button is-success" on:click={onSubmit}>Mint</button>
      <button class="button" on:click={onCancel}>Cancel</button>
    </footer>
  </div>
</div>