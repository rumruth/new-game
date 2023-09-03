<script>
  import { onMount } from 'svelte';

  let player_img = `./player.png`;
  let player_money = 0;
  let player_hunger = 0;
  let status = "";

  let timesSearched = 0;

  let storeItems = [
    "a bagel",
    "a Tesco Meal Deal",
    "some salmon meat",
    "a hat"
  ];

  function randomIntFromInterval(min, max) {
    return parseFloat((Math.random() * (min - max) + max).toFixed(2));
  }

  function searchCash() {
    if (timesSearched >= 10) {
      status = "You searched the streets... There was no cash. Try search again later.";
      return;
    }

    timesSearched++;
    let moneyFound = randomIntFromInterval(0.00, 1.00);

    status = "You found $" + moneyFound + "!";
    player_money += moneyFound;
    player_money = parseFloat(player_money.toFixed(2));
  }

  function browseReddit() {
    status = "You browse r/furry_irl. You feel shame and regret.";
  }

  function beGay() {
    status = "Being gay isn't a choice, but you try anyway. You don't feel any different.";
  }

  function bangWife() {
    status = "You ask your wife to bang... You realize you have no wife.";
  }

  function goToTescos() {
    let newStatus = "You went to Tescos. ";

    let itemCost = randomIntFromInterval(0.10, 10.00);
    let item = storeItems[Math.floor(Math.random() * storeItems.length)];

    newStatus += "You found " + item + " costing $" + itemCost + ". ";

    let cantAfford = player_money < itemCost;

    if (cantAfford) {
      newStatus += "You couldn't afford it. ";
    }
    else {
      player_money-= itemCost;
      player_money = parseFloat(player_money.toFixed(2));

      newStatus += "You bought " + item + ". ";

      if (item == "a hat") {
        storeItems.pop();
        player_img = "./player-hat.png";
        newStatus += " You look dapper.";
      }
      else {
        player_hunger = 0;
      }
    }

    status = newStatus;
  }

  onMount(() => {
    setInterval(() => {
      timesSearched = 0;
    }, 5000);

    setInterval(() => {
      player_hunger++;
    }, 5000);
  });
</script>

<main>
  <div class="player">
    <div class="player-icon">
      <img src={player_img}>
    </div>
    <div class="player-stats">
      <div class="player-stats-stat">
        <b>Money:</b> ${player_money}
      </div>
      <div class="player-stats-stat">
        <b>Hunger:</b> {player_hunger}
      </div>
      <div class="status">
        {status}
      </div>
    </div>
  </div>
  <div class="controls">
    <button on:click={searchCash}>Search the streets for change</button>
    <button on:click={goToTescos}>Go to Tescos</button>
    <button on:click={bangWife}>Bang your wife</button>
    <button on:click={beGay}>Be gay</button>
    <button on:click={browseReddit}>Browse r/furry_irl</button>
  </div>
</main>

<style lang="scss">
  main {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    padding: 20px;

    .player {
      display: flex;
      margin-bottom: 10px;

      &-icon {
        width: 150px;
        flex-shrink: 0;
        margin-right: 10px;
        border: 5px solid black;
        display: inline-flex;
        justify-content: center;
        align-items: center;

        img {
          width: 100%;
          height: 100%;
          align-self: center;
          object-fit: contain;
        }
      }

      &-stats {
        flex-grow: 1;
      }

      .status {
        width: 350px;
        max-width: 100%;
        padding: 10px 0 0;
      }
    }

    .controls {
      display: flex;
      flex-wrap: wrap;

      button {
        background: transparent;
        border: 2px solid black;
        border-radius: 0;
        cursor: pointer;
        transition: background 0.2s ease-out;
        padding: 3px 5px;
        margin: 0 5px 5px 0;
        display: inline-flex;

        &:hover {
          background: #dedede;
        }
      }
    }
  }
</style>
