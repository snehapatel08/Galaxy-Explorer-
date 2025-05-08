<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no" />
<title>Galaxy Explorer</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700&display=swap');

  :root {
    --primary-color: #7f00ff;
    --secondary-color: #00ffe7;
    --background-color: #0b0c10;
    --text-color: #cfd8dc;
    --accent-glow: #a259ff;
  }

  * {
    box-sizing: border-box;
  }

  html, body {
    margin: 0;
    height: 100%;
    font-family: 'Orbitron', sans-serif;
    background: var(--background-color);
    color: var(--text-color);
  }

  body {
    display: flex;
    flex-direction: column;
    max-width: 350px;
    max-height: 600px;
    margin: 0 auto;
    padding: 1rem;
    overflow: hidden;
  }

  header {
    text-align: center;
    margin-bottom: 1rem;
    flex-shrink: 0;
  }

  header h1 {
    font-size: 1.8rem;
    text-shadow:
      0 0 8px var(--accent-glow),
      0 0 20px var(--primary-color);
  }

  #galaxy-list {
    list-style: none;
    padding: 0;
    margin: 0 0 1rem 0;
    max-height: 220px;
    overflow-y: auto;
    border: 1px solid var(--accent-glow);
    border-radius: 8px;
    background: #121417;
    flex-shrink: 0;
  }

  #galaxy-list li {
    padding: 0.5rem 1rem;
    border-bottom: 1px solid #222;
    cursor: pointer;
    transition: background 0.3s;
    font-size: 1rem;
  }

  #galaxy-list li:hover,
  #galaxy-list li.active {
    background: var(--primary-color);
    color: #fff;
    box-shadow: 0 0 8px var(--accent-glow);
  }

  #galaxy-details {
    background: #121417;
    padding: 1rem;
    border-radius: 8px;
    box-shadow: 0 0 15px var(--accent-glow);
    flex-grow: 1;
    overflow-y: auto;
    min-height: 180px;
    display: flex;
    flex-direction: column;
    justify-content: center;
  }

  #galaxy-details h2 {
    margin-top: 0;
    margin-bottom: 0.5rem;
    color: var(--secondary-color);
    text-shadow: 0 0 10px var(--secondary-color);
    font-size: 1.3rem;
  }

  #galaxy-details p {
    line-height: 1.4;
    font-size: 1rem;
  }

  footer {
    text-align: center;
    font-size: 0.85rem;
    color: #444;
    margin-top: 1rem;
    flex-shrink: 0;
  }

  #galaxy-list::-webkit-scrollbar {
    width: 8px;
  }

  #galaxy-list::-webkit-scrollbar-track {
    background: #101214;
  }

  #galaxy-list::-webkit-scrollbar-thumb {
    background: var(--accent-glow);
    border-radius: 10px;
  }

  @media (max-width: 400px) {
    body {
      max-width: 100%;
      padding: 0.8rem;
    }
    header h1 {
      font-size: 1.4rem;
    }
    #galaxy-details h2 {
      font-size: 1.1rem;
    }
    #galaxy-details p,
    #galaxy-list li {
      font-size: 0.9rem;
    }
  }
</style>
</head>
<body>
  <header>
    <h1>Galaxy Explorer</h1>
    <p>Learn fascinating facts about nearby galaxies</p>
  </header>

  <ul id="galaxy-list" role="listbox" aria-label="List of galaxies">
    <li role="option" tabindex="0" data-id="1" class="active">Milky Way</li>
    <li role="option" tabindex="0" data-id="2">Andromeda</li>
    <li role="option" tabindex="0" data-id="3">Sombrero Galaxy</li>
    <li role="option" tabindex="0" data-id="4">Messier 87</li>
  </ul>

  <section id="galaxy-details" aria-live="polite" tabindex="0">
    <h2>Milky Way</h2>
    <p><strong>Type:</strong> Spiral</p>
    <p><strong>Distance from Earth:</strong> 0 (Home galaxy)</p>
    <p>The Milky Way is our home galaxy, containing our Solar System. It has a barred spiral shape and billions of stars.</p>
  </section>

  <footer>
    &copy; 2024 Galaxy Explorer. All rights reserved.
  </footer>

<script>
  // Static galaxy data embedded in JS object
  const galaxyData = {
    1: {
      name: 'Milky Way',
      type: 'Spiral',
      distance: '0 (Home galaxy)',
      description: 'The Milky Way is our home galaxy, containing our Solar System. It has a barred spiral shape and billions of stars.'
    },
    2: {
      name: 'Andromeda',
      type: 'Spiral',
      distance: '2.537 million light years',
      description: 'The Andromeda Galaxy is the closest spiral galaxy to the Milky Way and is on a collision course with our galaxy in about 4 billion years.'
    },
    3: {
      name: 'Sombrero Galaxy',
      type: 'Unbarred spiral',
      distance: '29.3 million light years',
      description: 'The Sombrero Galaxy is famous for its bright nucleus and large central bulge. It appears with a distinctive sombrero shape.'
    },
    4: {
      name: 'Messier 87',
      type: 'Elliptical',
      distance: '53.5 million light years',
      description: 'Messier 87 is a giant elliptical galaxy notable for its supermassive black hole, the first black hole ever imaged by the Event Horizon Telescope.'
    }
  };

  const galaxyListElem = document.getElementById('galaxy-list');
  const galaxyDetails = document.getElementById('galaxy-details');

  function escapeHtml(text) {
    return text.replace(/[&<>"']/g, (match) => {
      const escapeMap = {'&':'&amp;', '<':'&lt;', '>':'&gt;', '"':'&quot;', "'":'&#39;'};
      return escapeMap[match];
    });
  }

  function displayGalaxy(id) {
    const galaxy = galaxyData[id];
    if (!galaxy) return;
    galaxyDetails.innerHTML = `
      <h2>${escapeHtml(galaxy.name)}</h2>
      <p><strong>Type:</strong> ${escapeHtml(galaxy.type)}</p>
      <p><strong>Distance from Earth:</strong> ${escapeHtml(galaxy.distance)}</p>
      <p>${escapeHtml(galaxy.description)}</p>
    `;
    galaxyDetails.focus();
  }

  function handleSelection(target) {
    // Remove active class from all list items
    const items = galaxyListElem.querySelectorAll('li');
    items.forEach(item => item.classList.remove('active'));
    // Add active to selected item
    target.classList.add('active');
    // Display corresponding galaxy details
    const id = target.dataset.id;
    displayGalaxy(id);
  }

  galaxyListElem.addEventListener('click', (e) => {
    if (e.target && e.target.tagName === 'LI') {
      handleSelection(e.target);
    }
  });

  galaxyListElem.addEventListener('keydown', (e) => {
    if (e.target && e.target.tagName === 'LI') {
      if (e.key === 'Enter' || e.key === ' ') {
        e.preventDefault();
        handleSelection(e.target);
      }
    }
  });

  // Initialize with Milky Way selected
  window.onload = () => {
    const firstItem = galaxyListElem.querySelector('li.active') || galaxyListElem.querySelector('li');
    if (firstItem) {
      displayGalaxy(firstItem.dataset.id);
    }
  };
</script>
</body>
</html>
