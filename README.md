<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>My Diary</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<style>
  body {
    margin: 0;
    font-family: "Segoe UI", sans-serif;
    background: linear-gradient(180deg, #e8f4ff, #f6fbff);
    color: #2c3e50;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
  }

  .book {
    width: 90%;
    max-width: 600px;
    height: 85vh;
    background: #ffffff;
    border-radius: 18px;
    box-shadow: 0 20px 40px rgba(0,0,0,0.12);
    padding: 24px;
    display: flex;
    flex-direction: column;
  }

  .page {
    flex: 1;
    display: none;
    flex-direction: column;
  }

  .page.active {
    display: flex;
  }

  .cover {
    justify-content: center;
    align-items: center;
    text-align: center;
  }

  .cover h1 {
    font-size: 2.4em;
    margin-bottom: 10px;
    color: #4da3ff;
  }

  .cover p {
    opacity: 0.8;
    margin-bottom: 30px;
  }

  .enter-btn {
    padding: 14px 32px;
    font-size: 1em;
    border: none;
    border-radius: 30px;
    background: #4da3ff;
    color: white;
    cursor: pointer;
  }

  .enter-btn:hover {
    background: #3498ff;
  }

  .page h2 {
    margin: 0 0 10px;
    color: #4da3ff;
  }

  textarea {
    flex: 1;
    resize: none;
    border: none;
    outline: none;
    padding: 12px;
    font-size: 1em;
    background: #f4f9ff;
    border-radius: 12px;
    line-height: 1.6;
  }

  .nav {
    display: flex;
    justify-content: space-between;
    margin-top: 16px;
  }

  .nav button {
    padding: 10px 22px;
    border-radius: 20px;
    border: none;
    background: #4da3ff;
    color: white;
    cursor: pointer;
  }

  .nav button:disabled {
    background: #aacfff;
    cursor: not-allowed;
  }

  .page-num {
    text-align: center;
    font-size: 0.9em;
    opacity: 0.6;
    margin-top: 8px;
  }
</style>
</head>

<body>

<div class="book">

  <!-- Cover -->
  <div class="page cover active">
    <h1>My Diary</h1>
    <p>50 pages of thoughts, healing, and memories</p>
    <button class="enter-btn" onclick="goToPage(1)">Enter</button>
  </div>

  <!-- Pages -->
  <script>
    const totalPages = 50;
    for (let i = 1; i <= totalPages; i++) {
      document.write(`
        <div class="page">
          <h2>Day ${i}</h2>
          <textarea 
            data-page="${i}" 
            placeholder="Write your thoughts here..."
          ></textarea>
          <div class="nav">
            <button onclick="prevPage()" ${i === 1 ? "disabled" : ""}>Back</button>
            <button onclick="nextPage()" ${i === totalPages ? "disabled" : ""}>Next</button>
          </div>
          <div class="page-num">Page ${i} of ${totalPages}</div>
        </div>
      `);
    }
  </script>

</div>

<script>
  const pages = document.querySelectorAll(".page");
  const textareas = document.querySelectorAll("textarea");
  let currentPage = 0;

  // Restore saved text
  textareas.forEach(area => {
    const page = area.dataset.page;
    const saved = localStorage.getItem("diary_page_" + page);
    if (saved) area.value = saved;

    // Auto-save while typing
    area.addEventListener("input", () => {
      localStorage.setItem("diary_page_" + page, area.value);
    });
  });

  function showPage(index) {
    pages.forEach(p => p.classList.remove("active"));
    pages[index].classList.add("active");
    currentPage = index;
  }

  function nextPage() {
    if (currentPage < pages.length - 1) {
      showPage(currentPage + 1);
    }
  }

  function prevPage() {
    if (currentPage > 0) {
      showPage(currentPage - 1);
    }
  }

  function goToPage(index) {
    showPage(index);
  }
</script>

</body>
</html>
