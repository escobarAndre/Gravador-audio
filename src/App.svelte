<script>
  var mediaRecorder;
  var status = "";
  var record = 1;
  var records = []

  navigator.mediaDevices
    .getUserMedia({ audio: true, video: false })
    .then((stream) => {
      mediaRecorder = new MediaRecorder(stream);

      let chunks = [];

      mediaRecorder.ondataavailable = function (e) {
        chunks.push(e.data);
      };

      mediaRecorder.onstop = function (e) {
        const blob = new Blob(chunks, { type: "audio/ogg; codecs=opus" });

        chunks = [];

        const audioURL = URL.createObjectURL(blob);
        
        records = [
          ...records, {src: audioURL, gravação: record++}
        ]
        
      };
    });

  function start() {
    mediaRecorder.start();
    status = "Recording";
  }

  function stop() {
    mediaRecorder.stop();
    status = "";
  }

  function del(index) {
    records.splice(index, 1)
    records = records
    console.log('work')
  }

</script>

<main>
  <div id="record">
    <h1>Gravador de áudio</h1>
    <div id="btns">
      <button on:click={(start)}>Gravar</button>
      <button on:click={stop}>Parar</button>
      {#if status === "Recording"}
        <p id="recording">Gravando</p>
      {:else}
        <p>Aguardando</p>
      {/if}
    </div>
    <div id="AllRecords">
      {#each records as item, i}
        <div id="records" >
          <p>Gravação {item.gravação}</p>
          <audio controls src="{item.src}">
            <track kind="captions">
          </audio>
          <button on:click={() => del(i)}>Excluir</button>
        </div>
      {/each} 
    </div>
    <div id="AllRecords" />
  </div>
</main>

<style>
  button {
    margin: 0;
  }

  h1 {
    margin: 0 0 1rem 0;
  }

  #record {
    width: 60%;
    height: auto;
    padding: 1rem;
    left: 20%;
    position: absolute;
    top: 15%;
    border: 3px solid grey;
    text-align: center;
  }

  #record #recording {
    color: red;
    font-weight: 600;
  }

  #btns {
    display: flex;
    align-items: center;
    justify-content: space-evenly;
    width: 15rem;
    margin: 0 auto;
  }

  #AllRecords {
    display: inline;
  }

  #records {
    display: flex;
    align-items: center;
    justify-content: center;
    width: auto;
    height: auto;
    margin: 0 auto;
  }

  #record audio {
    margin: 0 1rem;
  }
</style>
