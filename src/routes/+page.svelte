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

  let activeFilter = 'all';

  let players: Player[] = [];
  let selectedPlayer: Player | null = null;

  function openPlayer(player: Player) {
    selectedPlayer = player;
  }

  function closePlayer() {
    selectedPlayer = null;
  }
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
  type Franchise = {
      owner: string;
      ownerImage: string;
      captain: string;
      team: string;
      color: string;
      logo: string;
    };

    const franchises: Franchise[] = [
      {
        owner: 'Ritanshu Thakur',
        ownerImage: '/photos/owners/Ritanshu Thakur.jpg',
        captain: 'Alankar Gaikar',
        team: 'Agri Royals',
        color: '#8b5cf6',
        logo: '/photos/logos/royals.png'
      },
      {
        owner: 'Mukesh Mahadik',
        ownerImage: '/photos/owners/Mukesh Mali.jpg',
        captain: 'Bhavesh Mahadik',
        team: 'Agri Thunders',
        color: '#38bdf8',
        logo: '/photos/logos/thunders.png'
      },
      {
        owner: 'Pranay Patil',
        ownerImage: '/photos/owners/Pranay Patil.jpg',
        captain: 'Mayur Mali',
        team: 'Agri Kings',
        color: '#facc15',
        logo: '/photos/logos/kings.png'
      },
      {
        owner: 'Mahendra Wavekar',
        ownerImage: '/photos/owners/Mahendra Wavekar.jpg',
        captain: 'Milind Pake',
        team: 'Agri Riders',
        color: '#14b8a6',
        logo: '/photos/logos/riders.png'
      },
      {
        owner: 'Prajyot Dhumal',
        ownerImage: '/photos/owners/Prajyot Dhumal.jpg',
        captain: 'Nikesh Sanas',
        team: 'Agri Stars',
        color: '#fb923c',
        logo: '/photos/logos/stars.png'
      },
      {
        owner: 'Vicky Mhatre',
        ownerImage: '/photos/owners/Vicky Mhatre.jpg',
        captain: 'Shailesh Mhatre',
        team: 'Agri Warriors',
        color: '#3b82f6',
        logo: '/photos/logos/warriors.png'
      },
      {
        owner: 'Manish Thakur',
        ownerImage: '/photos/owners/Manish Thakur.jpg',
        captain: 'Siddhesh Kudekar',
        team: 'Agri Titans',
        color: '#111111',
        logo: '/photos/logos/titans.png'
      },
      {
        owner: 'Manish Avkirkar',
        ownerImage: '/photos/owners/Manish Avkirkar.jpg',
        captain: 'Aniket Mhatre',
        team: 'Agri Spartans',
        color: '#ef4444',
        logo: '/photos/logos/spartans.png'
      }
    ];

    let selectedFranchise: Franchise | null = null;

    function openFranchise(franchise: Franchise) {
      selectedFranchise = franchise;
    }

    function closeFranchise() {
      selectedFranchise = null;
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
  <div class="live-banner">
    <div class="live-dot"></div>

    <div class="live-content">
      <span class="live-label">LIVE AUCTION</span>

      <div class="marquee">
        <div class="marquee-track">
          🏏 Live Auction on 24th May 2026 • Stay Tuned •
          Narli Agripada Premier League • APL 2026 •
          🏏 Live Auction on 24th May 2026 • Stay Tuned •
          Narli Agripada Premier League • APL 2026 •
        </div>
      </div>
    </div>
  </div>
  <div class="franchise-section">
    <div class="section-title">
      <span></span>
      OWNERS
    </div>

    <div class="franchise-grid">
      {#each franchises as franchise}
        <div
          class="franchise-card"
          style={`--team-color: ${franchise.color}`}
          on:click={() => openFranchise(franchise)}
        >
          <div class="team-glow"></div>

          <div class="card-top">
            <img
              src={franchise.logo}
              alt={franchise.team}
              class="team-logo"
            />

            <div class="team-name">
              {franchise.team}
            </div>
          </div>

          <div class="owner-preview">
            <img
              src={franchise.ownerImage}
              alt={franchise.owner}
              class="owner-preview-image"
            />
          </div>

          <div class="owner-preview-info">
            <div class="owner-title">
              TEAM OWNER
            </div>

            <div class="owner-preview-name">
              {franchise.owner}
            </div>
          </div>
        </div>
      {/each}
    </div>
  </div>


  <div class="filters">
    <button
        class:active-filter={activeFilter === 'all'}
        on:click={() => activeFilter = 'all'}
    >
        ALL
    </button>

    <button
        class:active-filter={activeFilter === 'captain'}
        on:click={() => activeFilter = 'captain'}
    >
        👑 CAPTAINS
    </button>

    <button
        class:active-filter={activeFilter === 'bowler'}
        on:click={() => activeFilter = 'bowler'}
    >
        🎯 BOWLERS
    </button>

    <button
        class:active-filter={activeFilter === 'batsman'}
        on:click={() => activeFilter = 'batsman'}
    >
        🏏 BATSMEN
    </button>

    <button
        class:active-filter={activeFilter === 'allrounder'}
        on:click={() => activeFilter = 'allrounder'}
    >
        ⚡ ALL ROUNDERS
    </button>
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
    
    {#each players.filter((player) => {
        if (activeFilter === 'all') return true;

        if (activeFilter === 'captain')
            return player.role.toLowerCase() === 'captain';

        if (activeFilter === 'bowler')
            return player.playerStyle.toLowerCase().includes('bowl');

        if (activeFilter === 'batsman')
            return player.playerStyle.toLowerCase().includes('bat');

        if (activeFilter === 'allrounder')
            return (
            player.playerStyle.toLowerCase().includes('all')
            );

        return true;
        }) as player, index}
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

<!--PLAYER OVERLAY -->
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
            on:error={(e) => handleImageError(e, selectedPlayer.role)}
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
<!--OWNER OVERLAY -->
{#if selectedFranchise}
  <div class="owner-overlay" on:click={() => closeFranchise()}>
    <div
      class="owner-modal"
      style={`--team-color: ${selectedFranchise.color}`}
      on:click|stopPropagation
    >
      <button
        type="button"
        class="owner-close"
        on:click={() => closeFranchise()}
      >
        ✕
      </button>

      <div class="animated-bg"></div>

      <div class="floating-logo-wrap">
        <img
          src={selectedFranchise.logo}
          alt={selectedFranchise.team}
          class="floating-team-logo"
        />
      </div>

      <div class="owner-content">

        <div class="owner-hero">
          <div class="owner-ring"></div>

          <img
            src={selectedFranchise.ownerImage}
            alt={selectedFranchise.owner}
            class="owner-image-large"
          />
        </div>

        <div class="owner-team-name">
          {selectedFranchise.team}
        </div>

        <div class="owner-name">
          {selectedFranchise.owner}
        </div>

        <div class="owner-role">
          TEAM OWNER
        </div>

        <div class="owner-divider"></div>

        <div class="captain-card">
          <img
            src={`/photos/captains/${selectedFranchise.captain}.jpg`}
            alt={selectedFranchise.captain}
            class="captain-small-image"
          />

          <div>
            <div class="captain-small-label">
              👑 RETAINED CAPTAIN
            </div>

            <div class="captain-small-name">
              {selectedFranchise.captain}
            </div>
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
    /* flex-direction: column; */
    gap: 10px;
    margin-bottom: 3px;
    padding: 4px;
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
    width: 35px;
    height: 35px;
    min-width: 35px;
    border-width: 2px;
  }

  .player-info {
    gap: 2px;
    flex: 1;
  }

  .name {
    font-size: 0.9rem;
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
  /* =========================
   MOBILE PLAYER LAYOUT
========================= */

/* hide dob */
.player-row > div:nth-child(3) {
  display: none !important;
}

/* hide size */
.player-row > div:nth-child(5) {
  display: none !important;
}

/* hide role badge */
.player-row > div:nth-child(6) {
  display: none !important;
}

/* top section */

.player-cell {
  display: flex;
  align-items: center;
  gap: 12px;
}

/* image */

.player-cell img {
  width: 35px;
  height: 35px;
  min-width: 35px;
}

/* info */

.player-info {
  flex: 1;
  min-width: 0;
}

.name {
  font-size: 0.9rem;
  line-height: 1.1;
  margin-bottom: 4px;
}

.sub {
  font-size: 0.68rem;
  letter-spacing: 1.5px;
}

/* style pill */

.player-row > div:nth-child(4) {
  margin-left: auto;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  padding: 6px 10px;
  min-width: 90px;
  border-radius: 999px;
  background: rgba(255, 255, 255, 0.04);
  border: 1px solid rgba(255, 255, 255, 0.04);
  font-size: 0.75rem;
  font-weight: 600;
  white-space: nowrap;
}

/* remove STYLE text */

.player-row > div:nth-child(4)::before {
  content: none;
}

/* captain glow */

.captain-row > div:nth-child(4),
.captain-row div:nth-child(4) {
  border-color: rgba(255, 191, 0, 0.25);
  box-shadow: 0 0 20px rgba(255, 191, 0, 0.08);
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
/* =========================
   FILTERS
========================= */

.filters {
  display: flex;
  gap: 12px;
  margin-bottom: 22px;
  overflow-x: auto;
  padding-bottom: 4px;
  position: relative;
  z-index: 2;
}

.filters::-webkit-scrollbar {
  display: none;
}

.filters button {
  border: none;
  background: rgba(255, 255, 255, 0.04);
  color: #9bcfff;
  padding: 12px 18px;
  border-radius: 999px;
  cursor: pointer;
  white-space: nowrap;
  font-weight: 700;
  letter-spacing: 1px;
  transition: 0.2s ease;
  border: 1px solid rgba(255, 255, 255, 0.05);
  backdrop-filter: blur(12px);
}

.filters button:hover {
  background: rgba(0, 162, 255, 0.08);
}

.active-filter {
  background: linear-gradient(
    135deg,
    rgba(0, 162, 255, 0.22),
    rgba(0, 162, 255, 0.08)
  ) !important;

  color: white !important;

  border: 1px solid rgba(0, 162, 255, 0.4) !important;

  box-shadow:
    0 0 20px rgba(0, 162, 255, 0.2),
    inset 0 0 20px rgba(255, 255, 255, 0.03);
}

/* captain active */

.filters button:nth-child(2).active-filter {
  background: linear-gradient(
    135deg,
    rgba(255, 191, 0, 0.22),
    rgba(255, 191, 0, 0.08)
  ) !important;

  border-color: rgba(255, 191, 0, 0.4) !important;

  box-shadow:
    0 0 20px rgba(255, 191, 0, 0.2),
    inset 0 0 20px rgba(255, 255, 255, 0.03);
}

/* MOBILE */

@media (max-width: 900px) {
  .filters {
    gap: 6px;
    margin-bottom: 16px;
    padding-inline: 2px;
  }

  .filters button {
    padding: 6px 10px;
    font-size: 0.68rem;
  }
}
/* =========================
   LIVE AUCTION BANNER
========================= */

.live-banner {
  position: relative;
  display: flex;
  align-items: center;
  gap: 16px;

  margin-bottom: 22px;
  padding: 16px 20px;

  border-radius: 22px;

  overflow: hidden;

  background:
    linear-gradient(
      135deg,
      rgba(0, 162, 255, 0.12),
      rgba(255, 0, 184, 0.08)
    );

  border: 1px solid rgba(0, 162, 255, 0.18);

  backdrop-filter: blur(16px);

  box-shadow:
    0 0 40px rgba(0, 162, 255, 0.08),
    inset 0 0 30px rgba(255, 255, 255, 0.02);
}

/* animated glow */

.live-banner::before {
  content: '';

  position: absolute;
  inset: 0;

  background:
    linear-gradient(
      90deg,
      transparent,
      rgba(255, 255, 255, 0.08),
      transparent
    );

  transform: translateX(-100%);

  animation: liveShine 4s linear infinite;
}

/* live dot */

.live-dot {
  width: 14px;
  height: 14px;
  min-width: 14px;

  border-radius: 50%;

  background: #ff3b3b;

  box-shadow:
    0 0 0 rgba(255, 59, 59, 0.7);

  animation: pulseLive 1.5s infinite;
}

/* content */

.live-content {
  min-width: 0;
  flex: 1;
}

.live-label {
  display: inline-block;

  margin-bottom: 6px;

  color: #ff7d7d;

  font-size: 0.72rem;
  font-weight: 800;

  letter-spacing: 2px;
}

/* marquee */

.marquee {
  overflow: hidden;
  white-space: nowrap;
  width: 100%;
}

.marquee-track {
  display: inline-block;

  color: white;

  font-weight: 700;
  letter-spacing: 1px;

  padding-left: 100%;

  animation: marqueeMove 28s linear infinite;
}

/* animations */

@keyframes marqueeMove {
  0% {
    transform: translateX(0%);
  }

  100% {
    transform: translateX(-100%);
  }
}

@keyframes pulseLive {
  0% {
    transform: scale(1);
    box-shadow:
      0 0 0 0 rgba(255, 59, 59, 0.7);
  }

  70% {
    transform: scale(1.08);
    box-shadow:
      0 0 0 12px rgba(255, 59, 59, 0);
  }

  100% {
    transform: scale(1);
    box-shadow:
      0 0 0 0 rgba(255, 59, 59, 0);
  }
}

@keyframes liveShine {
  100% {
    transform: translateX(100%);
  }
}

/* MOBILE */

@media (max-width: 900px) {
  .live-banner {
    padding: 14px 16px;
    gap: 12px;
    margin-bottom: 16px;
    border-radius: 18px;
  }

  .live-label {
    font-size: 0.66rem;
    margin-bottom: 4px;
  }

  .marquee-track {
    font-size: 0.85rem;
  }

  .live-dot {
    width: 12px;
    height: 12px;
    min-width: 12px;
  }
}
/* =========================
   FRANCHISE SECTION
========================= */

.franchise-section {
  margin-bottom: 26px;
  position: relative;
  z-index: 2;
}

.section-title {
  display: flex;
  align-items: center;
  gap: 12px;

  margin-bottom: 18px;

  font-size: 1rem;
  font-weight: 800;
  letter-spacing: 2px;

  color: white;
}

.section-title span {
  width: 40px;
  height: 3px;
  border-radius: 999px;
  background: linear-gradient(90deg, #00d0ff, #ff00b8);
}

.franchise-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 18px;
}
  /* =========================
     FRANCHISE SECTION
  ========================= */

  .franchise-section {
    margin-bottom: 26px;
    position: relative;
    z-index: 2;
  }

  .section-title {
    display: flex;
    align-items: center;
    gap: 12px;

    margin-bottom: 18px;

    font-size: 1rem;
    font-weight: 800;
    letter-spacing: 2px;

    color: white;
  }

  .section-title span {
    width: 40px;
    height: 3px;
    border-radius: 999px;
    background: linear-gradient(90deg, #00d0ff, #ff00b8);
  }

  .franchise-grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 18px;
  }

  /* CARD */

  .franchise-card {
    position: relative;

    overflow: hidden;

    border-radius: 26px;

    padding: 18px;

    background:
      linear-gradient(
        180deg,
        rgba(255,255,255,0.04),
        rgba(255,255,255,0.02)
      );

    border: 1px solid rgba(255,255,255,0.05);

    backdrop-filter: blur(16px);

    transition: 0.25s ease;

    isolation: isolate;

    cursor: pointer;

    text-align: left;
  }

  .franchise-card:hover {
    transform: translateY(-5px) scale(1.01);
  }

  .team-glow {
    position: absolute;

    inset: 0;

    background:
      radial-gradient(
        circle at top right,
        var(--team-color),
        transparent 70%
      );

    opacity: 0.18;

    z-index: -1;
  }

  .card-top {
    display: flex;
    align-items: center;
    justify-content: space-between;

    margin-bottom: 18px;
  }

  .team-logo {
    width: 62px;
    height: 62px;

    object-fit: contain;

    filter:
      drop-shadow(0 0 20px var(--team-color));
  }

  .team-name {
    font-size: 1.2rem;
    font-weight: 800;
  }

  .owner-preview {
    display: flex;
    justify-content: center;

    margin-bottom: 16px;
  }

  .owner-preview-image {
    width: 120px;
    height: 120px;

    object-fit: cover;

    border-radius: 50%;

    border: 3px solid var(--team-color);

    box-shadow:
      0 0 30px var(--team-color);

    transition: 0.25s ease;
  }

  .franchise-card:hover .owner-preview-image {
    transform: scale(1.05);
  }

  .owner-preview-info {
    text-align: center;
  }

  .owner-title {
    font-size: 0.68rem;
    letter-spacing: 2px;

    color: var(--team-color);

    margin-bottom: 8px;
  }

  .owner-preview-name {
    font-size: 1rem;
    font-weight: 700;
  }

  /* =========================
     OWNER OVERLAY
  ========================= */

  .owner-overlay {
    position: fixed;
    inset: 0;

    background: rgba(0,0,0,0.78);

    backdrop-filter: blur(14px);

    display: flex;
    justify-content: center;
    align-items: center;

    z-index: 9999;

    padding: 20px;

    animation: fadeIn 0.2s ease;
  }

  .owner-modal {
    position: relative;

    width: min(100%, 720px);

    overflow: hidden;

    border-radius: 34px;

    background:
      linear-gradient(
        180deg,
        rgba(12,18,42,0.98),
        rgba(5,8,20,0.99)
      );

    border: 1px solid rgba(255,255,255,0.06);

    box-shadow:
      0 0 80px rgba(0,0,0,0.5),
      0 0 120px var(--team-color);
  }

  .animated-bg {
    position: absolute;
    inset: 0;

    background:
      radial-gradient(
        circle at top,
        var(--team-color),
        transparent 70%
      );

    opacity: 0.22;

    animation: pulseBg 4s ease-in-out infinite;
  }

  .owner-close {
    position: absolute;

    top: 18px;
    right: 18px;

    width: 46px;
    height: 46px;

    border: none;
    border-radius: 50%;

    background: rgba(255,255,255,0.08);

    color: white;

    cursor: pointer;

    z-index: 20;
  }

  .floating-logo-wrap {
    position: absolute;

    top: 40px;
    right: 40px;

    animation: floatLogo 4s ease-in-out infinite;
  }

  .floating-team-logo {
    width: 90px;
    height: 90px;

    object-fit: contain;

    filter:
      drop-shadow(0 0 20px var(--team-color));
  }

  .owner-content {
    position: relative;
    z-index: 2;

    padding: 50px 34px 34px;

    text-align: center;
  }

  .owner-hero {
    position: relative;

    width: fit-content;

    margin:
      0 auto
      24px;
  }

  .owner-ring {
    position: absolute;

    inset: -12px;

    border-radius: 50%;

    border: 2px solid var(--team-color);

    opacity: 0.5;

    animation: rotateRing 12s linear infinite;
  }

  .owner-image-large {
    position: relative;

    width: 220px;
    height: 220px;

    object-fit: cover;

    border-radius: 50%;

    border: 4px solid var(--team-color);

    box-shadow:
      0 0 60px var(--team-color);

    animation: ownerFloat 4s ease-in-out infinite;
  }

  .owner-team-name {
    font-size: 2.5rem;
    font-weight: 800;

    margin-bottom: 8px;
  }

  .owner-name {
    font-size: 1.2rem;

    color: #dce8ff;

    margin-bottom: 14px;
  }

  .owner-role {
    display: inline-flex;

    padding: 10px 18px;

    border-radius: 999px;

    background: rgba(255,255,255,0.05);

    border: 1px solid rgba(255,255,255,0.08);

    color: var(--team-color);

    font-size: 0.75rem;
    font-weight: 800;
    letter-spacing: 2px;
  }

  .owner-divider {
    width: 100%;
    height: 1px;

    margin:
      30px 0;

    background:
      linear-gradient(
        90deg,
        transparent,
        rgba(255,255,255,0.1),
        transparent
      );
  }

  /* captain */

  .captain-card {
    display: inline-flex;
    align-items: center;
    gap: 16px;

    padding: 16px 20px;

    border-radius: 22px;

    background: rgba(255,255,255,0.04);

    border: 1px solid rgba(255,255,255,0.06);
  }

  .captain-small-image {
    width: 70px;
    height: 70px;

    object-fit: cover;

    border-radius: 50%;

    border: 2px solid var(--team-color);

    box-shadow:
      0 0 24px var(--team-color);
  }

  .captain-small-label {
    font-size: 0.65rem;
    letter-spacing: 2px;

    color: var(--team-color);

    margin-bottom: 6px;
  }

  .captain-small-name {
    font-size: 1rem;
    font-weight: 700;
  }

  /* animations */

  @keyframes floatLogo {
    0%,100% {
      transform: translateY(0px);
    }

    50% {
      transform: translateY(-10px);
    }
  }

  @keyframes ownerFloat {
    0%,100% {
      transform: translateY(0px);
    }

    50% {
      transform: translateY(-8px);
    }
  }

  @keyframes pulseBg {
    0%,100% {
      opacity: 0.18;
    }

    50% {
      opacity: 0.28;
    }
  }

  @keyframes rotateRing {
    from {
      transform: rotate(0deg);
    }

    to {
      transform: rotate(360deg);
    }
  }

  /* MOBILE */

  @media (max-width: 900px) {
    /* =========================
   MOBILE FRANCHISE GRID
========================= */

    .franchise-grid {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 12px;
    }

    /* card */

    .franchise-card {
      min-width: 0;

      padding: 4px;

      border-radius: 20px;
    }

    /* top */

    .card-top {
      margin-bottom: 4px;
    }

    .team-logo {
      width: 20px;
      height: 20px;
    }

    .team-name {
      font-size: 0.75rem;
    }

    /* owner */

    .owner-preview {
      margin-bottom: 4px;
    }

    .owner-preview-image {
      width: 45px;
      height: 45px;

      border-width: 2px;
    }

    .owner-title {
      font-size: 0.56rem;
      letter-spacing: 1px;

      margin-bottom: 2px;
    }

    .owner-preview-name {
      font-size: 0.72rem;
      line-height: 1.2;
    }

    .owner-overlay {
      padding: 0;
      align-items: flex-end;
    }

    .owner-modal {
      width: 100%;
      height: 100dvh;

      border-radius: 28px 28px 0 0;

      overflow-y: auto;
    }

    .floating-logo-wrap {
      top: 18px;
      right: 18px;
    }

    .floating-team-logo {
      width: 64px;
      height: 64px;
    }

    .owner-content {
      padding: 60px 18px 30px;
    }

    .owner-image-large {
      width: 170px;
      height: 170px;
    }

    .owner-team-name {
      font-size: 2rem;
    }

    .captain-card {
      width: 100%;

      justify-content: center;

      padding: 14px;
    }

    .captain-small-image {
      width: 58px;
      height: 58px;
    }
  }

</style>