<script lang="ts">
  import { onMount } from 'svelte';

  type Player = {
    playerName: string;
    dateOfBirth: string;
    playerStyle: string;
    tshirtSize: string;
    role: string;
    photo: string;
  };

  let players: Player[] = [];
  let selectedPlayer: Player | null = null;

  // CSV Loader
  function handleImageError(event: Event, role: string) {
    const target = event.target as HTMLImageElement;

    target.src =
        role.toLowerCase() === 'captain'
        ? '/photos/default-captain.png'
        : '/photos/default-player.png';
    }
  async function loadCSV() {
    const response = await fetch('/player.csv');
    const text = await response.text();

    const rows = text.split('\n').slice(1);

    players = rows
      .filter((r) => r.trim())
      .map((row) => {
        const cols = row.split(',');

        const playerName = cols[0]?.trim();
        const dateOfBirth = cols[1]?.trim();
        const playerStyle = cols[3]?.trim();
        const tshirtSize = cols[4]?.trim();
        const role = cols[5]?.trim();

        return {
          playerName,
          dateOfBirth,
          playerStyle,
          tshirtSize,
          role,
          photo:
            role.toLowerCase() === 'captain'
                ? `/photos/captains/${playerName}.jpg`
                : `/photos/players/${playerName}.jpg`
        };
      });
  }

  onMount(() => {
    loadCSV();
  });

  function openPlayer(player: Player) {
    selectedPlayer = player;
  }

  function closePlayer() {
    selectedPlayer = null;
  }
</script>

<svelte:head>
  <title>Players</title>
</svelte:head>

<div class="page">
  <div class="bg-glow glow-1"></div>
  <div class="bg-glow glow-2"></div>

<div class="header">
  <div class="league-brand">
    <img src="/apl_2026_logo.png" alt="APL Logo" class="league-logo" />

    <div class="title-wrap">
      <div class="mini-line"></div>

      <h1>APL</h1>

      <div class="league-full-name">
        Narli Agripada Premier League
      </div>

      <span>CRICKET AUCTION • TEAM LEGENDS</span>
    </div>
  </div>
</div>
  <div class="table-container">
    <div class="table-header">
      <div>#</div>
      <div>PLAYER</div>
      <div>D.O.B</div>
      <div>STYLE</div>
      <div>SIZE</div>
      <div>ROLE</div>
    </div>

    {#each players as player, index}
      <button
        class:captain-row={player.role.toLowerCase() === 'captain'}
        class="player-row"
        on:click={() => openPlayer(player)}
      >
        <div class="index">{index + 1}</div>

        <div class="player-cell">
          <img
            src={player.photo}
            alt={player.playerName}
            on:error={(e) => handleImageError(e, player.role)}
            />
          <div class="player-info">
            <span class="name">{player.playerName}</span>
            <span class="sub">
              {player.role.toUpperCase()}
            </span>
          </div>
        </div>

        <div>{player.dateOfBirth}</div>

        <div>&nbsp; {player.playerStyle}</div>

        <div>{player.tshirtSize}</div>

        <div>
          {#if player.role.toLowerCase() === 'captain'}
            <span class="captain-badge">
              👑 CAPTAIN
            </span>
          {:else}
            <span class="player-badge">
              PLAYER
            </span>
          {/if}
        </div>
      </button>
    {/each}
  </div>
</div>

<!-- OVERLAY -->
{#if selectedPlayer}
  <div class="overlay" on:click={closePlayer}>
    <div class="modal" on:click|stopPropagation>
      <button class="close-btn" on:click={closePlayer}>
        ✕
      </button>

      <div class="modal-top">
        <div
          class:selected-captain={selectedPlayer.role.toLowerCase() === 'captain'}
          class="photo-frame"
        >
          <img
            src={selectedPlayer.photo}
            alt={selectedPlayer.playerName}
          />
        </div>
      </div>

      <div class="details">
        <h2>{selectedPlayer.playerName}</h2>

        {#if selectedPlayer.role.toLowerCase() === 'captain'}
          <div class="overlay-role captain">
            👑 CAPTAIN
          </div>
        {:else}
          <div class="overlay-role player">
            PLAYER
          </div>
        {/if}

        <div class="detail-grid">
          <div class="detail-card">
            <span class="label">DATE OF BIRTH</span>
            <span>{selectedPlayer.dateOfBirth}</span>
          </div>

          <div class="detail-card">
            <span class="label">PLAYER STYLE</span>
            <span>{selectedPlayer.playerStyle}</span>
          </div>

          <div class="detail-card">
            <span class="label">TSHIRT SIZE</span>
            <span>{selectedPlayer.tshirtSize}</span>
          </div>
        </div>
      </div>
    </div>
  </div>
{/if}

<style>
  :global(body) {
    margin: 0;
    background: #040816;
    font-family: Inter, sans-serif;
    color: white;
    overflow-x: hidden;
  }

  * {
    box-sizing: border-box;
  }

  .page {
    min-height: 100vh;
    padding: 40px;
    position: relative;
    overflow: hidden;
    background:
      radial-gradient(circle at top left, #101d48 0%, transparent 30%),
      radial-gradient(circle at bottom right, #3a005c 0%, transparent 30%),
      #040816;
  }

  .bg-glow {
    position: fixed;
    width: 500px;
    height: 500px;
    border-radius: 50%;
    filter: blur(120px);
    opacity: 0.15;
    pointer-events: none;
    z-index: 0;
  }

  .glow-1 {
    background: #00a2ff;
    top: -100px;
    left: -100px;
  }

  .glow-2 {
    background: #ff00c8;
    bottom: -100px;
    right: -100px;
  }

  /* HEADER */

  .header {
    display: flex;
    justify-content: center;
    margin-bottom: 40px;
    position: relative;
    z-index: 1;
  }

  .league-brand {
    display: flex;
    align-items: center;
    gap: 28px;
    padding: 18px 32px;
    border-radius: 30px;
    background: rgba(255, 255, 255, 0.03);
    border: 1px solid rgba(0, 162, 255, 0.12);
    backdrop-filter: blur(14px);
    box-shadow:
      0 0 40px rgba(0, 162, 255, 0.12),
      inset 0 0 30px rgba(255, 255, 255, 0.02);
  }

  .league-logo {
    width: 110px;
    height: 110px;
    object-fit: contain;
    filter:
      drop-shadow(0 0 12px rgba(0, 162, 255, 0.5))
      drop-shadow(0 0 30px rgba(255, 0, 200, 0.25));
  }

  .title-wrap {
    text-align: left;
  }

  .mini-line {
    width: 100px;
    height: 4px;
    border-radius: 999px;
    background: linear-gradient(90deg, #00d0ff, #ff00b8);
    margin-bottom: 18px;
    box-shadow: 0 0 20px #00d0ff;
  }

  h1 {
    margin: 0;
    font-size: 4rem;
    letter-spacing: 6px;
    line-height: 1;
    background: linear-gradient(90deg, #ffffff, #00a2ff);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
  }

  .league-full-name {
    font-size: 1rem;
    letter-spacing: 4px;
    font-weight: 700;
    text-transform: uppercase;
    margin-top: 10px;
    background: linear-gradient(90deg, #ffffff, #79cfff);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
  }

  .title-wrap span {
    display: inline-block;
    margin-top: 12px;
    color: #7cb7ff;
    letter-spacing: 3px;
    font-size: 0.85rem;
  }

  /* TABLE */

  .table-container {
    position: relative;
    z-index: 1;
    border: 1px solid rgba(0, 170, 255, 0.35);
    background: rgba(5, 12, 35, 0.72);
    backdrop-filter: blur(18px);
    border-radius: 30px;
    overflow: hidden;
    box-shadow:
      0 0 40px rgba(0, 128, 255, 0.2),
      inset 0 0 20px rgba(255, 255, 255, 0.03);
  }

  .table-header,
  .player-row {
    display: grid;
    grid-template-columns: 70px 2fr 1.2fr 1.5fr 100px 180px;
    align-items: center;
  }

  .table-header {
    padding: 24px 28px;
    color: #68bfff;
    font-size: 0.9rem;
    font-weight: 700;
    border-bottom: 1px solid rgba(255, 255, 255, 0.06);
    background: rgba(0, 0, 0, 0.25);
    letter-spacing: 1px;
  }

  .player-row {
    width: 100%;
    border: none;
    background: transparent;
    color: white;
    padding: 18px 28px;
    cursor: pointer;
    transition: 0.25s ease;
    border-bottom: 1px solid rgba(255, 255, 255, 0.04);
    text-align: left;
  }

  .player-row:hover {
    background: rgba(0, 162, 255, 0.08);
    transform: scale(1.01);
  }

  .captain-row {
    background: linear-gradient(
      90deg,
      rgba(255, 187, 0, 0.08),
      rgba(255, 187, 0, 0.02)
    );
    box-shadow: inset 0 0 40px rgba(255, 196, 0, 0.08);
  }

  .index {
    font-size: 2rem;
    font-weight: 800;
    color: #00a2ff;
  }

  .captain-row .index {
    color: #ffd54a;
  }

  .player-cell {
    display: flex;
    align-items: center;
    gap: 18px;
  }

  .player-cell img {
    width: 72px;
    height: 72px;
    object-fit: cover;
    border-radius: 50%;
    border: 2px solid #00a2ff;
    box-shadow: 0 0 25px rgba(0, 162, 255, 0.45);
    background: #071224;
  }

  .captain-row img {
    border-color: #ffd54a;
    box-shadow: 0 0 30px rgba(255, 200, 0, 0.6);
  }

  .player-info {
    display: flex;
    flex-direction: column;
    gap: 5px;
  }

  .name {
    font-size: 1.2rem;
    font-weight: 700;
  }

  .sub {
    color: #7ca5d8;
    font-size: 0.8rem;
    letter-spacing: 2px;
  }

  .captain-badge,
  .player-badge {
    padding: 12px 18px;
    border-radius: 999px;
    font-weight: 700;
    display: inline-flex;
    align-items: center;
    justify-content: center;
  }

  .captain-badge {
    background: rgba(255, 191, 0, 0.12);
    color: #ffd54a;
    border: 1px solid rgba(255, 191, 0, 0.4);
    box-shadow: 0 0 20px rgba(255, 191, 0, 0.3);
  }

  .player-badge {
    background: rgba(0, 162, 255, 0.1);
    color: #4fc3ff;
    border: 1px solid rgba(0, 162, 255, 0.35);
  }

  /* OVERLAY */

  .overlay {
    position: fixed;
    inset: 0;
    background: rgba(0, 0, 0, 0.72);
    backdrop-filter: blur(12px);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 999;
    padding: 20px;
    animation: fadeIn 0.2s ease;
  }

  .modal {
    width: min(100%, 850px);
    background: linear-gradient(
      180deg,
      rgba(10, 16, 42, 0.96),
      rgba(4, 8, 22, 0.99)
    );
    border-radius: 30px;
    border: 1px solid rgba(0, 162, 255, 0.25);
    overflow: hidden;
    position: relative;
    box-shadow:
      0 0 80px rgba(0, 162, 255, 0.25),
      0 0 120px rgba(255, 0, 200, 0.12);
  }

  .modal::-webkit-scrollbar {
    width: 0;
  }

  .modal {
    -ms-overflow-style: none;
    scrollbar-width: none;
  }

  .close-btn {
    position: absolute;
    top: 18px;
    right: 18px;
    width: 46px;
    height: 46px;
    border-radius: 50%;
    border: none;
    background: rgba(255, 255, 255, 0.08);
    color: white;
    cursor: pointer;
    font-size: 1rem;
    backdrop-filter: blur(10px);
  }

  .modal-top {
    padding: 50px 40px 10px;
    display: flex;
    justify-content: center;
  }

  .photo-frame {
    width: 320px;
    height: 420px;
    border-radius: 28px;
    overflow: hidden;
    border: 2px solid #00a2ff;
    box-shadow: 0 0 50px rgba(0, 162, 255, 0.45);
    background: #071224;
  }

  .selected-captain {
    border-color: #ffd54a;
    box-shadow: 0 0 60px rgba(255, 196, 0, 0.5);
  }

  .photo-frame img {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }

  .details {
    padding: 30px 40px 45px;
    text-align: center;
  }

  .details h2 {
    margin: 0;
    font-size: 3rem;
    margin-bottom: 12px;
    line-height: 1.1;
  }

  .overlay-role {
    display: inline-flex;
    padding: 12px 22px;
    border-radius: 999px;
    margin-bottom: 35px;
    font-weight: 700;
    letter-spacing: 1px;
  }

  .overlay-role.captain {
    background: rgba(255, 191, 0, 0.12);
    color: #ffd54a;
    border: 1px solid rgba(255, 191, 0, 0.4);
  }

  .overlay-role.player {
    background: rgba(0, 162, 255, 0.1);
    color: #4fc3ff;
    border: 1px solid rgba(0, 162, 255, 0.35);
  }

  .detail-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 18px;
  }

  .detail-card {
    background: rgba(255, 255, 255, 0.04);
    border: 1px solid rgba(255, 255, 255, 0.05);
    border-radius: 20px;
    padding: 24px;
    display: flex;
    flex-direction: column;
    gap: 10px;
  }

  .label {
    font-size: 0.75rem;
    color: #7cb7ff;
    letter-spacing: 2px;
  }

  @keyframes fadeIn {
    from {
      opacity: 0;
      transform: scale(0.96);
    }

    to {
      opacity: 1;
      transform: scale(1);
    }
  }

  @keyframes mobileModal {
    from {
      transform: translateY(100%);
    }

    to {
      transform: translateY(0%);
    }
  }

  /* MOBILE */

@media (max-width: 900px) {
  .page {
    padding: 10px;
  }

  /* =========================
     HEADER
  ========================= */

  h1 {
    font-size: 2.2rem;
    letter-spacing: 2px;
  }

  .header {
    margin-bottom: 18px;
  }

  .league-brand {
    flex-direction: row;
    align-items: center;
    gap: 14px;
    padding: 14px;
    border-radius: 20px;
  }

  .league-logo {
    width: 64px;
    height: 64px;
    flex-shrink: 0;
  }

  .title-wrap {
    text-align: left;
    flex: 1;
  }

  .mini-line {
    width: 60px;
    margin-bottom: 10px;
    margin-inline: 0;
  }

  .league-full-name {
    font-size: 0.72rem;
    letter-spacing: 1px;
    line-height: 1.3;
    margin-top: 6px;
  }

  .title-wrap span {
    display: none;
  }

  /* =========================
     TABLE
  ========================= */

  .table-container {
    border-radius: 18px;
    overflow: hidden;
  }

  .table-header {
    display: none;
  }

  .player-row {
    display: flex;
    flex-direction: column;
    gap: 10px;
    padding: 14px;
    transform: none !important;
    border-bottom: 1px solid rgba(255, 255, 255, 0.04);
  }

  .player-row:hover {
    transform: none;
    background: rgba(0, 162, 255, 0.04);
  }

  .index {
    display: none;
  }

  /* =========================
     PLAYER TOP SECTION
  ========================= */

  .player-cell {
    display: flex;
    align-items: center;
    gap: 12px;
  }

  .player-cell img {
    width: 58px;
    height: 58px;
    min-width: 58px;
    border-width: 2px;
  }

  .player-info {
    gap: 2px;
    flex: 1;
  }

  .name {
    font-size: 1rem;
    line-height: 1.15;
  }

  .sub {
    font-size: 0.68rem;
    letter-spacing: 1.5px;
  }

  /* =========================
     HIDE UNNECESSARY DATA
  ========================= */

  /* DOB */
  .player-row > div:nth-child(3) {
    display: none !important;
  }

  /* SIZE */
  .player-row > div:nth-child(5) {
    display: none !important;
  }

  /* =========================
     STYLE CARD
  ========================= */

  .player-row > div:nth-child(4) {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 10px 14px;
    border-radius: 14px;
    background: rgba(255, 255, 255, 0.03);
    font-size: 0.88rem;
  }

  .player-row > div:nth-child(4)::before {
    content: 'STYLE - ';
    color: #72bbff;
    font-size: 0.68rem;
    letter-spacing: 1px;
  }

  /* =========================
     BADGES
  ========================= */

  .captain-badge,
  .player-badge {
    width: 100%;
    justify-content: center;
    min-height: 44px;
    font-size: 0.84rem;
    padding: 10px 14px;
    border-radius: 14px;
  }

  /* =========================
     OVERLAY
  ========================= */

  .overlay {
    padding: 0;
    align-items: flex-end;
    overflow: hidden;
  }

  .modal {
    width: 100%;
    height: 100dvh;
    max-height: 100dvh;
    border-radius: 24px 24px 0 0;
    overflow-y: auto;
    overflow-x: hidden;
    animation: mobileModal 0.22s ease;
    padding-bottom: 30px;
  }

  .modal::-webkit-scrollbar {
    width: 0;
  }

  .close-btn {
    position: sticky;
    top: 14px;
    margin-left: auto;
    margin-right: 14px;
    width: 42px;
    height: 42px;
    z-index: 50;
    backdrop-filter: blur(10px);
  }

  .modal-top {
    padding: 0 16px;
  }

  .photo-frame {
    width: 100%;
    max-width: 290px;
    height: 360px;
    border-radius: 20px;
  }

  .details {
    padding: 22px 16px 30px;
  }

  .details h2 {
    font-size: 1.9rem;
    line-height: 1.1;
    margin-bottom: 10px;
  }

  .overlay-role {
    margin-bottom: 22px;
    font-size: 0.85rem;
    padding: 10px 16px;
  }

  /* =========================
     DETAIL GRID
  ========================= */

  .detail-grid {
    grid-template-columns: 1fr;
    gap: 12px;
  }

  .detail-card {
    padding: 16px;
    text-align: left;
    border-radius: 16px;
  }

  .label {
    font-size: 0.68rem;
    letter-spacing: 1px;
  }
}
</style>