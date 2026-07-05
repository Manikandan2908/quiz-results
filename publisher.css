  /* ── STATE ── */
  var studentName = '', studentSchool = '', studentDistrict = '';
  var currentSlide = 0;
  var userAnswers  = {};
  var slides       = [];
  var totalSlides  = 0;

  /* ── BUILD OPTION BUTTONS with shuffle ── */
  function buildOptionButtons() {
    slides      = document.querySelectorAll('.slide');
    totalSlides = slides.length;

    slides.forEach(function(slide, idx) {
      var sel         = slide.querySelector('select');
      var optionsList = slide.querySelector('.options-list');
      optionsList.innerHTML = '';
      var labels = ['A','B','C','D'];

      var options = Array.from(sel.options).map(function(opt) {
        return { value: opt.value, text: opt.textContent.trim(), correct: opt.dataset.correct || 'false' };
      });

      // Fisher-Yates shuffle
      for (var i = options.length - 1; i > 0; i--) {
        var j = Math.floor(Math.random() * (i + 1));
        var tmp = options[i]; options[i] = options[j]; options[j] = tmp;
      }

      options.forEach(function(opt, oi) {
        var btn = document.createElement('button');
        btn.type            = 'button';
        btn.className       = 'opt-btn';
        btn.dataset.value   = opt.value;
        btn.dataset.correct = opt.correct;
        btn.innerHTML       = '<span class="opt-badge">' + labels[oi] + '</span><span>' + opt.text + '</span>';
        btn.addEventListener('click', function() { selectOption(idx, opt.value, btn); });
        optionsList.appendChild(btn);
      });
    });
  }

  /* ── SELECT OPTION ── */
  function selectOption(slideIdx, value, clickedBtn) {
    userAnswers[slideIdx] = value;

    var slide = slides[slideIdx];
    slide.querySelectorAll('.opt-btn').forEach(function(b) { b.classList.remove('selected'); });
    clickedBtn.classList.add('selected');

    updateUI();
  }

  /* ── NAVIGATE ── */
  function navigate(dir) {
    if (dir === 1 && userAnswers[currentSlide] === undefined) {
      shakeSlide();
      return;
    }
    var next = currentSlide + dir;
    if (next < 0 || next >= totalSlides) return;
    currentSlide = next;
    updateUI();
  }

  function shakeSlide() {
    var active = document.querySelector('.slide.active-slide');
    if (!active) return;
    active.style.animation = 'shake 0.4s';
    setTimeout(function() { active.style.animation = ''; }, 400);
  }

  /* ── UPDATE UI ── */
  function updateUI() {
    var answered = Object.keys(userAnswers).length;

    /* slide visibility */
    slides.forEach(function(slide, idx) {
      if (idx === currentSlide) {
        slide.classList.add('active-slide');
      } else {
        slide.classList.remove('active-slide');
      }
    });

    /* progress bar */
    var pct = Math.round((answered / totalSlides) * 100);
    document.getElementById('progressFill').style.width    = pct + '%';
    document.getElementById('progressLabel').textContent   = 'Question ' + (currentSlide + 1) + ' of ' + totalSlides;
    document.getElementById('progressPct').textContent     = pct + '%';

    /* prev button */
    document.getElementById('prevBtn').style.display = currentSlide > 0 ? 'inline-flex' : 'none';

    /* remaining hint */
    var remaining = totalSlides - answered;
    var hint = document.getElementById('remainingHint');
    if (remaining > 0) {
      hint.textContent  = remaining + ' left';
      hint.style.color  = 'var(--border)';
    } else {
      hint.textContent  = 'All done ';
      hint.style.color  = 'var(--emerald)';
    }

    /* Next button morphs into Submit on last slide when all answered */
    var nextBtn      = document.getElementById('nextBtn');
    var onLastSlide  = (currentSlide === totalSlides - 1);
    var allAnswered  = (answered === totalSlides);

    if (onLastSlide && allAnswered) {
      nextBtn.innerHTML  = '<i class="fa fa-check-circle"></i> Submit Quiz';
      nextBtn.className  = 'btn btn-primary';
      nextBtn.onclick    = submitQuiz;
    } else {
      nextBtn.innerHTML  = 'Next <i class="fa fa-arrow-right"></i>';
      nextBtn.className  = 'btn btn-gold';
      nextBtn.onclick    = function() { navigate(1); };
    }
  }

  /* ── INITIATE QUIZ ── */
  function initiateQuiz() {
    studentName     = document.getElementById('name').value.trim();
    studentSchool   = document.getElementById('school').value.trim();
    studentDistrict = document.getElementById('district').value.trim();

    if (!studentName || !studentSchool || !studentDistrict) {
      alert('Please fill in all required fields before starting the quiz.');
      return;
    }

    buildOptionButtons();

    document.getElementById('regSection').style.display = 'none';
    document.getElementById('quizArea').style.display   = 'block';

    currentSlide = 0;
    userAnswers  = {};

    updateUI();
  }

  /* ── SUBMIT ── */
  function submitQuiz() {
    var correctCount = 0;

    slides.forEach(function(slide, idx) {
      var userVal = userAnswers[idx];
      slide.querySelectorAll('.opt-btn').forEach(function(btn) {
        if (btn.dataset.correct === 'true' && btn.dataset.value === userVal) {
          correctCount++;
        }
      });
    });

    var total = totalSlides;
    var pct   = (correctCount / total) * 100;

    saveResult(studentName, studentSchool, studentDistrict, correctCount, total, pct);
    renderReview(correctCount, total, pct);

    document.getElementById('quizArea').style.display     = 'none';
    document.getElementById('reviewSection').style.display = 'block';

    if (pct >= 50) {
      setTimeout(function() { showCertificate(correctCount, total, pct); }, 800);
    }
  }

  /* ── RENDER REVIEW ── */
  function renderReview(correctCount, total, pct) {
    var rev    = document.getElementById('reviewSection');
    var labels = ['A','B','C','D'];

    var html = '<div class="score-banner">'
      + '<div class="score-num">' + correctCount + ' / ' + total + '</div>'
      + '<div class="score-label">Your Score</div>'
      + '<div class="score-pct">' + pct.toFixed(1) + '%' + (pct >= 50 ? ' 🏆 Certificate Earned!' : '') + '</div>'
      + '</div>';

    slides.forEach(function(slide, idx) {
      var qText       = slide.querySelector('.q-text').textContent;
      var qTa         = slide.querySelector('.q-text-ta').textContent;
      var optBtns     = slide.querySelectorAll('.opt-btn');
      var userVal     = userAnswers[idx];
      var explanation = slide.dataset.explanation;

      var isCorrect = false;
      optBtns.forEach(function(btn) {
        if (btn.dataset.correct === 'true' && btn.dataset.value === userVal) isCorrect = true;
      });

      html += '<div class="review-item ' + (isCorrect ? 'correct' : 'wrong') + '">'
        + '<div class="review-item-header">'
        + '<span class="verdict-badge ' + (isCorrect ? 'verdict-correct' : 'verdict-wrong') + '">' + (isCorrect ? '✓ Correct' : '✗ Wrong') + '</span>'
        + '<strong>Q' + (idx + 1) + '.</strong> ' + qText
        + '<div class="q-ta">' + qTa + '</div>'
        + '</div><div class="review-options">';

      optBtns.forEach(function(btn, oi) {
        var isCorrectOpt = btn.dataset.correct === 'true';
        var isUserPick   = btn.dataset.value === userVal;
        var cls  = isCorrectOpt ? 'opt-correct' : (isUserPick ? 'opt-wrong-pick' : '');
        var icon = isCorrectOpt ? '✓' : (isUserPick ? '✗' : labels[oi]);
        html += '<div class="review-opt ' + cls + '">'
          + '<span class="opt-icon">' + icon + '</span>'
          + '<span>' + btn.querySelector('span:last-child').textContent + '</span>'
          + '</div>';
      });

      html += '</div>';

      if (explanation) {
        html += '<div style="margin:8px 18px 18px;padding:12px 16px;background:rgba(201,168,76,0.12);border-left:4px solid var(--gold);border-radius:6px;font-size:0.92rem;color:var(--ink);">'
          + '<strong>Explanation:</strong> ' + explanation + '</div>';
      }

      html += '</div>';
    });

    html += '<div class="retry-row">'
      + '<button class="btn btn-outline" onclick="location.reload()"><i class="fa fa-refresh"></i> Retry Quiz</button>'
      + '<a href="https://www.icteducationtools.com/2026/04/nmms-test-series-collection-2026.html" class="btn btn-primary" style="text-decoration:none;" target="_blank">More Tests</a>'
      + '</div>';

    rev.innerHTML = html;
  }

  /* ── CERTIFICATE ── */
  function showCertificate(correctCount, total, pct) {
    document.getElementById('certName').textContent     = studentName;
    document.getElementById('certSchool').textContent   = studentSchool;
    document.getElementById('certDistrict').textContent = studentDistrict;
    document.getElementById('certScore').textContent    = correctCount + ' out of ' + total + ' (' + pct.toFixed(1) + '%)';
    var d = new Date();
    document.getElementById('certDate').textContent = d.toLocaleDateString('en-IN', { day:'numeric', month:'long', year:'numeric' });
    document.getElementById('certificateModal').style.display = 'flex';
  }

  function closeCertificate() {
    document.getElementById('certificateModal').style.display = 'none';
  }

  function downloadCertificate() {
    var certificate = document.getElementById('certificate');
    var safeName    = studentName.replace(/\s+/g, '_');
    html2canvas(certificate, { scale: 2, backgroundColor: '#ffffff', logging: false, useCORS: true })
      .then(function(canvas) {
        var link      = document.createElement('a');
        link.download = 'Certificate_' + safeName + '.jpg';
        link.href     = canvas.toDataURL('image/jpeg', 0.95);
        link.click();
      });
  }

