# BUILD "КОЛОБОК" — ONE-PROMPT SPEC

Build a single-page promo site for a fictional film. Every asset already exists —
download it, do not generate anything. Match the reference implementation pixel for pixel.

**Reference implementation (open it and compare as you go):**
https://kolobok-vdali-ot-sborov.vercel.app

═══════════════════════════════════════════════════════════════════════════
0. HOW TO WORK
═══════════════════════════════════════════════════════════════════════════
You need a coding agent with a shell and network access (Claude Code, Codex CLI,
Cursor, Windsurf, Copilot agent mode …). A chat window with no terminal cannot
fetch the assets and will not finish this task.

1. Fetch every asset first — one command, nothing to type by hand:

       BASE=https://storage.yandexcloud.net/german-gc-assets/kolobok
       curl -fsS "$BASE/manifest.txt" -o manifest.txt
       while read -r f; do
         mkdir -p "$(dirname "$f")"
         curl -fsS -o "$f" "$BASE/$f"
       done < manifest.txt
       rm manifest.txt
       find assets fonts -type f | wc -l    # must print 27

   Mirror, if that host is unreachable from your network — same paths, same files:

       BASE=https://kolobok-vdali-ot-sborov.vercel.app

2. Write three files: `index.html`, `styles.css`, `hero.js`. No frameworks, no build step,
   no npm. All motion runs in one requestAnimationFrame loop.
3. Serve locally over a server that supports HTTP Range (otherwise the video cannot seek).
   Python's `http.server` does NOT support Range — write a tiny Range-capable server,
   or use any static server that does.
4. Verify yourself against §12 before reporting done.

All Russian copy below is final. Copy it verbatim, character for character.
Never translate it, never rewrite it, never "improve" the punctuation.

═══════════════════════════════════════════════════════════════════════════
1. IDENTITY
═══════════════════════════════════════════════════════════════════════════
Flat photographic sky / glossy inflatable 3D objects / puffy balloon typography /
a children's movie poster delivering a dry adult joke / everything floats and scatters on scroll.

Premise: a studio made a flop and is honest about it. Copy tone is deadpan.

═══════════════════════════════════════════════════════════════════════════
2. STACK
═══════════════════════════════════════════════════════════════════════════
index.html + styles.css + hero.js. Zero dependencies. No React, no Tailwind, no GSAP,
no Lenis, no scroll libraries.

One rAF loop drives everything, with frame-time correction:
    k = 1 - Math.pow(0.0016, dt)      // dt in seconds, capped at 0.1
    smooth += (window.scrollY - smooth) * k
Without the correction the inertia runs twice as fast on 120 Hz displays.

Cache layout metrics (offsetTop, offsetHeight, viewport size). Never call
getBoundingClientRect inside the frame. Recompute on resize, on load, and from a
ResizeObserver on document.body — images arrive after load and shift the layout.

═══════════════════════════════════════════════════════════════════════════
3. ASSETS — download, do not generate
═══════════════════════════════════════════════════════════════════════════
BASE = https://storage.yandexcloud.net/german-gc-assets/kolobok
MIRROR = https://kolobok-vdali-ot-sborov.vercel.app

Both hosts serve identical files at identical paths. `$BASE/manifest.txt` lists all 27.
Save each file to the same relative path shown below.

    assets/sky.jpg                      2880×1608  sky background, clean centre
    assets/island.webp                  1600×1600  floating island with the bun character
    assets/vdali.webp                    900×604   word "ВДАЛИ", yellow balloon letters
    assets/otsborov.webp                1500×636   word "ОТ СБОРОВ", chrome letters
    assets/cloud.webp                   1300×872   single cumulus cloud, cut out
    assets/cinema.webp                  2000×1131  cinema hall, screen area cut out (alpha)
    assets/preloader-ball.webp           700×700   the bun character, front view, square
    assets/letters3/ltr-k1.webp          500×872   К   ┐
    assets/letters3/ltr-o1.webp          500×822   О   │
    assets/letters3/ltr-l1.webp          500×815   Л   │ the seven letters of the
    assets/letters3/ltr-o2.webp          500×814   О   │ headline, already separated
    assets/letters3/ltr-b1.webp          500×902   Б   │
    assets/letters3/ltr-o3.webp          500×818   О   │
    assets/letters3/ltr-k2.webp          500×816   К   ┘
    assets/blocks/spider.webp            800×980   masked hero figure tangled in web
    assets/blocks/grandparents.webp      850×1062  elderly couple waving
    assets/blocks/oleg.webp             1100×1287  3D character in a black hat
    assets/blocks/ico_reel.webp          520×591   film reel icon
    assets/blocks/ico_seat.webp          520×710   cinema seat icon
    assets/blocks/ico_ticket.webp        520×440   ticket icon
    assets/blocks/film-strip.webp       1900×1900  film strip winding in an S-shape
    assets/blocks/hl-boxoffice.webp     1100×457   headline "ПРОКАТ В ЦИФРАХ"
    assets/blocks/mask-on.webp           160×209   red hero mask, active state
    assets/blocks/mask-off.webp          160×209   same mask, grey, inactive state
    assets/blocks/finale-poster.jpg      960×536   first frame of the finale video
    assets/blocks/finale-video.mp4      1928×1076  10 s, the bun rolls away over the horizon
    fonts/coolvetica-rg.ttf                        the only typeface on the site

File names must stay ASCII. Cyrillic in a path turns into percent-encoding and behaves
differently across hosts — that is why the letters are named ltr-k1 … ltr-k2.

═══════════════════════════════════════════════════════════════════════════
4. TYPE
═══════════════════════════════════════════════════════════════════════════
@font-face { font-family: 'Coolvetica'; src: url('fonts/coolvetica-rg.ttf') format('truetype');
             font-weight: 400; font-display: swap; }
body { font-family: 'Coolvetica', 'Helvetica Neue', Arial, sans-serif; }

One typeface across the whole page. Never introduce a second one.

═══════════════════════════════════════════════════════════════════════════
5. TOKENS
═══════════════════════════════════════════════════════════════════════════
:root {
  --u: calc(100vw / 1440);   /* layout unit: every size is calc(N * var(--u)) */
  --lh-display: 0.89;
  --ink: #123049;
  --yellow: #FFD400;
}
body { background: #79C0F2; overflow-x: clip; min-height: 100%; }
* { margin: 0; padding: 0; box-sizing: border-box; }
html { scroll-behavior: smooth; }

N is the pixel value measured on a 1440-wide artboard. Do not convert to px or rem.

═══════════════════════════════════════════════════════════════════════════
6. DOM TREE
═══════════════════════════════════════════════════════════════════════════
.pre#pre                       z 200  preloader curtain
.sky                           z 0    position: fixed, inset: 0 — sky under everything
header.topbar                  z 100  fixed; .menu left, a.trailer right
main
  section.hero#hero            z 1    height 162vh
    .hero__stage                      position: fixed, inset: 0, pointer-events: none
      .word#word                      seven .word__letter divs, injected by JS
      .island-group#islandGroup       img.island + 3 .grass-shadow + img.vdali + img.otsborov
      img.cloud.cloud--left
      img.cloud.cloud--right
  section.about#about          z 1    min-height 100vh, flex centre
  section.reviews#reviews      z 1    padding 22vh 0 30vh
  section.boxoffice#numbers    z 1    min-height 190vh
  div.cinema-wrap#cinemaWrap   z 12   height 300vh
    .cinema-pin                       position: sticky, top 0, height 100vh, overflow hidden
      .cinema-screen#cinemaScreen     the hole in the hall image; JS sets left/top/w/h
        .cinema-track#cinemaTrack     three .cinslide, flex row
      img.cinema-hall#cinemaHall      object-fit cover, transform-origin 50% 32%
  section.hero-sec#hero-about  z 1    min-height 130vh
  section.finale#finale        z 11   height 100vh
    .finale__pin                      position: relative, height 100vh, overflow hidden
      .finale__sky / video.finale__bg#finaleVideo / .finale__copy
.bottom-blur                   z 90   fixed bottom strip
.release / a.booking / .budget#budget   z 100  fixed

Order matters: .sky sits under the content, the fixed UI sits above every section.

═══════════════════════════════════════════════════════════════════════════
7. MATERIALS
═══════════════════════════════════════════════════════════════════════════
glass        background rgba(255,255,255,.1); backdrop-filter blur(calc(16 * var(--u)));
             border-radius 999px. No borders, ever.
yellow button
             background:
               radial-gradient(120% 150% at 50% -30%, rgba(255,255,255,.95) 0%, rgba(255,255,255,0) 46%),
               linear-gradient(180deg, #FFE566 0%, var(--yellow) 42%, #E3A600 100%);
             box-shadow:
               inset 0 calc(2 * var(--u)) calc(3 * var(--u)) rgba(255,255,255,.9),
               inset 0 calc(-6 * var(--u)) calc(12 * var(--u)) rgba(180,120,0,.45),
               0 calc(10 * var(--u)) calc(26 * var(--u)) rgba(18,48,73,.22);
             border-radius 999px; color #6a4b00.
white card   background #fff; border-radius calc(22 * var(--u));
             box-shadow 0 calc(18 * var(--u)) calc(46 * var(--u)) rgba(18,48,73,.18).
tag          border-radius calc(12 * var(--u)) — a rounded rectangle, NOT a pill;
             a pill reads as a button.
bottom blur  height calc(230 * var(--u)); backdrop-filter blur(calc(22 * var(--u)));
             mask-image linear-gradient(to top, #000 0%, transparent 78%).
             Legibility comes from blur only — not one dark pixel anywhere.
cursor       @media (pointer: fine) { body, a, button { cursor: url('data:image/svg+xml;utf8,
             <svg xmlns="http://www.w3.org/2000/svg" width="30" height="30" viewBox="0 0 30 30">
             <path d="M5 3 L26 13.5 L16.6 15.8 L21.3 25.2 L16.5 27.4 L12 17.8 L5.5 23 Z"
             fill="%23ffd000" stroke="%235c3d10" stroke-width="2.4" stroke-linejoin="round"/>
             </svg>') 4 3, auto; } }

═══════════════════════════════════════════════════════════════════════════
8. SECTIONS — exact values
═══════════════════════════════════════════════════════════════════════════

── SKY ──
.sky { position: fixed; inset: 0; z-index: 0; background-color: #4AA3E8;
       background: url('assets/sky.jpg') center center / 152% auto no-repeat; }
152% pushes the clouds off the edges and keeps the middle clean for content.

── TOP BAR ──
.topbar  fixed; top calc(28 * var(--u)); left/right calc(48 * var(--u));
         display flex; justify-content space-between; gap calc(24 * var(--u))
.menu    display flex; gap calc(22 * var(--u))
.menu a  font-size calc(15 * var(--u)); line-height 1; letter-spacing .01em;
         color #fff; opacity .82 → 1 on hover. No background, no shadow.
         Items, verbatim:  О фильме  /  Отзывы  /  Прокат  /  Герой
         Anchors: #about, #reviews, #numbers, #hero-about
.trailer glass; padding calc(10 * var(--u)) calc(20 * var(--u)) calc(10 * var(--u))
         calc(15 * var(--u)); font-size calc(15 * var(--u)); a 16u play triangle in front;
         label verbatim:  Трейлер

── FIXED BOTTOM UI ──
.release  fixed; bottom calc(34 * var(--u)); left calc(48 * var(--u));
          display flex; gap calc(10 * var(--u)); font-size calc(18 * var(--u)); color #fff
          ticket icon 22u, fill #fff, stroke #79C0F2 1.6
          text verbatim:  В кино с 6 августа
.booking  fixed; bottom calc(34 * var(--u)); left 50%; transform translateX(-50%);
          yellow button; padding calc(15 * var(--u)) calc(38 * var(--u)) calc(17 * var(--u));
          flex column; gap calc(3 * var(--u))
          title  font-size calc(21 * var(--u)) — Забронировать весь зал
          note   font-size calc(13 * var(--u)); opacity .58 — Свободно, мы проверяли
          hover: translateY(calc(-3 * var(--u))) and a deeper shadow
.budget   fixed; bottom calc(34 * var(--u)); right calc(48 * var(--u));
          flex column; align-items flex-end; gap calc(6 * var(--u)); color #fff
          label  font-size calc(14 * var(--u)); letter-spacing .12em; uppercase; opacity .8
                 verbatim:  Бюджет сайта
          value  font-size calc(34 * var(--u)); an odometer, see §9

── HERO ──
The headline is images, there is no text in the HTML.

JS builds seven .word__letter divs from the source bounding boxes,
measured on the original 4096×2751 lettering:
    К   x199   y783   w713  h1243        ltr-k1.webp
    О1  x800   y895   w555  h912         ltr-o1.webp
    Л   x1300  y854   w583  h950         ltr-l1.webp
    О2  x1836  y895   w555  h904         ltr-o2.webp
    Б   x2376  y845   w534  h963         ltr-b1.webp
    О3  x2853  y895   w558  h913         ltr-o3.webp
    К2  x3366  y894   w583  h952         ltr-k2.webp

    const K_SCALE = 1269.01 / 4096;      // source → artboard
    const K_OX = 136 - 38.95;            // crop offset X
    const K_OY = 135 - 243.16;           // crop offset Y
    const U = 1 / 1440;
    left   = (K_OX + box.x * K_SCALE) * U * 100 + 'vw'
    top    = (K_OY + box.y * K_SCALE) * U * 100 + 'vw'   // vw on purpose, not vh
    width  = box.w * K_SCALE * U * 100 + 'vw'
    height = box.h * K_SCALE * U * 100 + 'vw'

.island        left calc(193 * var(--u)); top calc(176 * var(--u));
               width calc(1021 * var(--u)); height calc(1022 * var(--u))
.vdali         left calc(472 * var(--u)); top calc(555 * var(--u));
               width calc(463 * var(--u)); height calc(311 * var(--u))
.otsborov      left calc(259 * var(--u)); top calc(602 * var(--u));
               width calc(898 * var(--u)); height calc(381 * var(--u))
.island-group  transform-origin 50% 26%

Three ellipse shadows on the grass, background #14330f, mix-blend-mode multiply:
    wide   left 412u  top 812u  w 579u  h 40u   opacity .34  blur 14u
    short  left 405u  top 802u  w 164u  h 40u   opacity .30  blur 12u
    soft   left 488u  top 670u  w 432u  h 105u  opacity .22  blur 26u

.cloud         width calc(910 * var(--u)); height calc(611 * var(--u))
.cloud--left   left calc(-538 * var(--u)); top calc(-43 * var(--u))
.cloud--right  left calc(966 * var(--u));  top calc(231 * var(--u))

── ABOUT ──
.about        min-height 100vh; flex; centred; overflow-x clip
.about__cloud width calc(620 * var(--u)); absolute
              left cloud  left calc(-90 * var(--u)); top calc(120 * var(--u))
              right cloud right calc(-70 * var(--u)); top calc(300 * var(--u))
.about__hero--spider        left calc(60 * var(--u)); top calc(30 * var(--u));
                            width calc(330 * var(--u))
.about__hero--grandparents  right calc(70 * var(--u)); top calc(190 * var(--u));
                            width calc(340 * var(--u))
.about__text  width calc(600 * var(--u)); text-align centre; color #fff; z-index 2

Copy, verbatim:
    label   О фильме
    line 1  Он ушёл от бабушки.
    line 2  Ушёл от дедушки.
    small   От сравнений с Человеком-пауком уйти не смог.
    tags    Жанр — семейная сказка   (yellow)
            Возраст — 6+             (white)

.section-label       font-size calc(15 * var(--u)); letter-spacing .22em; uppercase;
                     color #fff; opacity .75
.about__line         font-size calc(54 * var(--u)); line-height 0.97;
                     letter-spacing -.025em; text-wrap balance;
                     margin-top calc(4 * var(--u))
.about__line--small  font-size calc(26 * var(--u)); line-height 1.25; opacity .85;
                     margin-top calc(24 * var(--u))
.tags                flex; justify-content centre; gap calc(12 * var(--u));
                     margin-top calc(40 * var(--u))
.tag                 padding calc(12 * var(--u)) calc(20 * var(--u)) calc(13 * var(--u));
                     font-size calc(17 * var(--u)); color var(--ink)

── REVIEWS ──
.reviews         padding 22vh 0 30vh; overflow-x clip
.reviews__intro  width calc(700 * var(--u)); margin 0 auto calc(70 * var(--u)); centred; #fff
.reviews__title  font-size calc(64 * var(--u)); line-height 0.95; letter-spacing -.025em;
                 margin-top calc(14 * var(--u))
.reviews__lead   font-size calc(20 * var(--u)); line-height 1.4; opacity .82;
                 margin-top calc(20 * var(--u))
.reviews__list   width calc(760 * var(--u)); margin 0 auto; flex column; gap 26vh
.review          width calc(560 * var(--u));
                 padding calc(26 * var(--u)) calc(30 * var(--u)) calc(22 * var(--u));
                 white card
.review__badge   absolute; top calc(-14 * var(--u)); left calc(26 * var(--u));
                 padding calc(7 * var(--u)) calc(16 * var(--u)) calc(8 * var(--u));
                 border-radius 999px; background var(--yellow); color #6a4b00;
                 font-size calc(13 * var(--u)); white-space nowrap
.review__avatar  38u circle, flex centre, colour #fff, font-size calc(19 * var(--u))
.review__name    font-size calc(17 * var(--u))
.review__body    margin-top calc(16 * var(--u)); font-size calc(21 * var(--u)); line-height 1.35
.review__likes   margin-top calc(20 * var(--u)); font-size calc(16 * var(--u)); opacity .5;
                 a 19u thumbs-up glyph before the number

Copy, verbatim:
    label  Отзывы
    title  Что говорят люди
    lead   Обычно здесь цитируют критиков.<br>У нас — настоящие зрители, с настоящими лайками.

    card 1  badge: Самый залайканный отзыв о фильме
            Дмитрий Кравцов · avatar Д · gradient 135deg #F2A65A → #D97B2B
            «Нет, хейт недостаточен»
            7 478          tilt -2.4deg   align-self flex-start
    card 2  Артём Валеев · avatar А · gradient 135deg #6FB1E8 → #3D7FC1
            «Человек-паук сражался с осьминогом, носорогом, стервятником, песком,
            водой и электричеством. А одолел его хлебный мякиш»
            1 126          tilt  1.8deg   align-self flex-end
    card 3  Ольга Терентьева · avatar О · gradient 135deg #8ED081 → #57A44B
            «Теперь я рад, что в конце сказки Колобка съела лиса»
            1 458          tilt -1.4deg   align-self flex-start, margin-left calc(60 * var(--u))
    card 4  Павел Ким · avatar П · gradient 135deg #E8879B → #C2566E
            «— Можно сделать нормально? — Можно, а зачем?»
            312            tilt  2.6deg   align-self flex-end, margin-right calc(40 * var(--u))

(the "·" above is a separator in this spec only — never put it on the page itself)

── BOX OFFICE ──
.boxoffice     min-height 190vh; flex column; align-items centre; padding 12vh 0 10vh;
               color #fff; overflow-x clip
.bo__strip     absolute; left 50%; top 4vh; z-index 0;
               width min(102vw, calc(1500 * var(--u)));
               mask-image linear-gradient(to bottom, #000 86%, transparent 99%)
               — without the mask the bottom edge of the strip hangs in the sky as a raw cut
.bo__headline  align-self flex-start; margin-left 6vw;
               width min(calc(560 * var(--u)), 40vw); z-index 2
.bofact        flex column; align-items centre; text-align centre;
               max-width calc(380 * var(--u)); z-index 2
    fact 1     align-self flex-end;   margin 6vh 7vw 0 0
    fact 2     align-self flex-start; margin 7vh 0 0 14vw
    fact 3     align-self flex-end;   margin 8vh 18vw 0 0
.bofact__icon  width clamp(calc(120 * var(--u)), 12vw, calc(180 * var(--u))); aspect-ratio 1;
               drop-shadow(0 calc(14 * var(--u)) calc(20 * var(--u)) rgba(18,48,73,.28))
.bofact b      font-size clamp(calc(64 * var(--u)), 7.4vw, calc(118 * var(--u)));
               line-height 1; letter-spacing -.03em; font-variant-numeric tabular-nums;
               margin-top calc(10 * var(--u))
.bofact i      font-size clamp(calc(15 * var(--u)), 1.3vw, calc(19 * var(--u)));
               line-height 1.35; opacity .92; text-wrap balance; font-style normal;
               margin-top calc(8 * var(--u))

Copy, verbatim (data-count drives the count-up):
    2171  копий — больше, чем у любого фильма этим летом        ico_reel
    11    человек в среднем зале на сто пятьдесят мест          ico_seat
    4     билета на девять сеансов за день в одном московском кинотеатре   ico_ticket

── CINEMA ──
.cinema-wrap    height 300vh; z-index 12
.cinema-pin     position sticky; top 0; height 100vh; overflow hidden
.cinema-hall    absolute inset 0; width/height 100%; object-fit cover;
                transform scale(2.6); transform-origin 50% 32%
.cinema-screen  absolute; overflow hidden; display flex; colour #fff — JS positions it
.cinema-track   flex; width/height 100%;
                transition transform .65s cubic-bezier(.22, .61, .36, 1)
.cinslide       flex 0 0 100%; flex column; centred; gap calc(14 * var(--u));
                text-align centre; padding 0 6%
.cinslide b     font-size clamp(calc(34 * var(--u)), 3.6vw, calc(58 * var(--u)));
                letter-spacing -.02em; line-height var(--lh-display); text-wrap balance
.cinslide i     font-size clamp(calc(15 * var(--u)), 1.3vw, calc(20 * var(--u)));
                opacity .9; font-style normal
.cinlogo        font-size clamp(calc(13 * var(--u)), 1.15vw, calc(17 * var(--u)));
                padding .32em .7em; border-radius .34em; line-height 1.15
                --imdb  background #f5c518; colour #0b0b0b; font-weight 700
                --kp    background #ff5500; colour #fff

The screen hole inside assets/cinema.webp (measured on the 2688×1520 original):
    x 617 … 2112,  y 272 … 1051
JS computes its on-screen position under object-fit: cover:
    s  = Math.max(vw / 2688, vh / 1520)
    ox = (vw - 2688 * s) / 2
    oy = (vh - 1520 * s) / 2
    left = 617 * s + ox;  top = 272 * s + oy
    width = (2112 - 617) * s;  height = (1051 - 272) * s
The sky of the page shows through the hole. That is intended.

Slides, verbatim:
    1  IMDb        1 балл из 10 на IMDb
                   ten stars, exactly one filled (#ffd000, rest rgba(255,255,255,.28), 22–34px)
                   Меньше поставить технически невозможно. Мы проверили.
    2  Кинопоиск   Кинопоиск спрятал рейтинг
                   "7,4" behind a black censor bar: ::after, height .42em,
                   transform translateY(-50%) rotate(-1.2deg), background #0b1220
                   За ночь прилетели тысячи оценок
    3  Кинопоиск   1,9 паука из 10
                   ten mask cells; mask-on.webp clipped over mask-off.webp with
                   clip-path: inset(0 calc((1 - var(--fill)) * 100%) 0 0);
                   fills: 1, .9, then eight zeros
                   Кинопоиск заменил звёзды на масках Человека-паука

── HERO SECTION (character) ──
.hero-sec         min-height 130vh; flex centred; padding 10vh calc(40 * var(--u)) 30vh;
                  background linear-gradient(to bottom,
                    rgba(240,240,242,0) 0%, rgba(240,240,242,.06) 30%,
                    rgba(240,240,242,.2) 52%, rgba(240,240,242,.45) 68%,
                    rgba(240,240,242,.72) 82%, rgba(240,240,242,.92) 92%, #f0f0f2 100%)
                  — the sky has to arrive at the video's top tone before the next section
.hero-sec__inner  flex; align-items centre; gap calc(70 * var(--u));
                  max-width calc(1160 * var(--u))
.hero-sec__mongol width calc(520 * var(--u));
                  drop-shadow(calc(-18 * var(--u)) calc(26 * var(--u)) calc(44 * var(--u))
                  rgba(20,50,90,.35))
.hero-sec__title  font-size clamp(calc(40 * var(--u)), 4.6vw, calc(72 * var(--u)));
                  line-height var(--lh-display); letter-spacing -.025em
.hero-sec__sub    margin-top calc(24 * var(--u));
                  font-size clamp(calc(17 * var(--u)), 1.5vw, calc(22 * var(--u)));
                  line-height 1.42; opacity .95
.review--cloud    margin-top calc(46 * var(--u)); max-width calc(520 * var(--u))

Copy, verbatim:
    label     О герое
    title     Круглый. Румяный.<br>Ни в чём не виноват.
    sub       Внешность героя не имеет отношения<br>к Олегу Монголу. Он просил передать.<br>Несколько раз.
    card      Зритель · avatar З · gradient 135deg #d8c8ff → #6a4ab8 · tilt -1.4deg
              «Надеюсь, они выплачивают Олегу Монголу роялти за использование его лица»
              1 697

── FINALE ──
.finale        height 100vh; z-index 11;
               background linear-gradient(to bottom, #f0f0f2 0%, #e6ecf3 22%,
               #dbe7f2 46%, #d4e4f2 100%)
               — mandatory: without it the blue fixed sky shows through the video mask
                 and draws a hard line across the seam
.finale__pin   position relative; height 100vh; overflow hidden; flex column; centred
.finale__sky   absolute; top -1px; height 46vh; z-index 3;
               linear-gradient(to bottom, rgba(240,240,242,.85) 0%,
               rgba(240,240,242,.5) 30%, rgba(240,240,242,.18) 60%, rgba(240,240,242,0) 100%)
.finale__bg    absolute; top 0; left 0; width 100%; height 118%; object-fit cover;
               object-position 50% 0%;
               mask-image linear-gradient(to bottom,
                 rgba(0,0,0,0) 0%, rgba(0,0,0,.06) 10%, rgba(0,0,0,.2) 20%,
                 rgba(0,0,0,.45) 30%, rgba(0,0,0,.72) 40%, rgba(0,0,0,.92) 50%, #000 60%)
               attributes: muted playsinline autoplay preload="auto"
               poster="assets/blocks/finale-poster.jpg"
.finale__copy  position relative; z-index 4; text-align centre; colour var(--ink);
               margin-top 24vh
.finale__lead  font-size clamp(calc(28 * var(--u)), 2.9vw, calc(46 * var(--u)));
               line-height var(--lh-display); letter-spacing -.015em
.finale__zero  font-size clamp(calc(90 * var(--u)), 10vw, calc(160 * var(--u)));
               line-height .95; letter-spacing -.04em; margin-top calc(10 * var(--u))
.finale__note  margin-top calc(14 * var(--u));
               font-size clamp(calc(14 * var(--u)), 1.2vw, calc(18 * var(--u))); opacity .92

Copy, verbatim:
    Здесь заканчивался<br>бюджет сайта
    0 ₽
    Всё остальное ушло в продакшн фильма

═══════════════════════════════════════════════════════════════════════════
9. MOTION
═══════════════════════════════════════════════════════════════════════════

── PRELOADER ──
Full-screen curtain, z 200,
background radial-gradient(120% 90% at 50% 42%, #a9d9f7 0%, #6fb6ee 38%, #2f7fd0 78%, #1c5fa8 100%);
transition opacity .45s, visibility .45s.

    .pre__ball  preloader-ball.webp, left 50% top 46%, translate(-50%,-50%),
                width clamp(150px, 22vh, 260px), rotate 0 → 360deg in 2.4s linear infinite
    .pre__glow  62vh circle behind it, radial white .5 → 0 at 68%,
                scale 1 → 1.1 and opacity .9 → 1, 2.6s ease-in-out infinite
    .pre__rail  1px vertical line, right clamp(24px,4vw,64px), inset top/bottom clamp(24px,5vh,60px)
    .pre__num   bottom: calc(var(--p, 0) * (100% - 1em)); transform translateY(50%);
                font-size clamp(38px, 5vw, 74px); tabular-nums;
                a 12×2px white tick at ::after; "%" at .42em, opacity .7, vertical-align super;
                transition bottom .35s

Progress is real: preload the sky, island, cloud, vdali, otsborov and the seven letters,
count how many resolved, and let the displayed number chase the real one:
    shown += (real - shown) * 0.12
Without the chase it jumps from 0 to 100 on a warm cache.
When everything is in: set 100, wait 420 ms, add .is-done, and 700 ms later set
display: none (if the opacity transition never ran, the curtain would hang over the scene).
Hard timeout: release after 9 s no matter what.

── INTRO — starts only after the curtain leaves ──
If the clock starts earlier, the animation plays under the curtain and the viewer
gets a fully assembled screen.

    seg(t, a, b)   = clamp01((t - a) / (b - a))
    easeOutBack(t) = 1 + 2.2 * (t-1)^3 + 1.2 * (t-1)^2
    easeOutExpo(t) = t >= 1 ? 1 : 1 - 2^(-10t)

    iCloud = easeOutExpo(seg(t, 0, 2.2))
    iTitle = seg(t, 0.9, 2.1)
    iIsle  = easeOutBack(seg(t, 1.05, 2.6))
    iWord  = seg(t, 1.9, 3.2)
    hover  = Math.sin(t * 1.1) * 0.5                      // permanent island levitation

    clouds     left:  translate((30*(1-iCloud) - 16*p1)vw, (7*(1-iCloud) - 24*p1)vh)
               right: translate((-30*(1-iCloud) + 20*p1)vw, (-7*(1-iCloud) - 20*p1)vh)
               both:  scale(1.12 - 0.12 * iCloud)
    letters    up = easeOutBack(seg(iTitle, i*0.07, i*0.07 + 0.62))
               translateY((1-up) * 11 vh), rotate((1-up) * (i-3) * 5 deg),
               scale(0.88 + 0.12*up), opacity = up * (fade from scroll)
    island     translateY((1 - iIsle) * 85 vh + hover)
    words      translateY((1 - easeOutBack(iWord)) * 5 vh), scale(0.9 + 0.1*…), opacity

Drive the clock from performance.now(), not from a frame counter — a backgrounded tab
must still end up in the assembled state. Also run a 4.5 s safety interval that calls
the apply() function every 100 ms in case rAF never fires.

── HERO ON SCROLL ──
    p1 = clamp01(position / (1.1 * viewportH))        // letters, linear, from pixel one
    p3 = easeInOut(clamp01(position / (1.4 * viewportH)))   // island group

    letter fly vectors (x in vw, y in vh, rotation in degrees):
        К  -46 -34 -38     О1 -30 -52 -24     Л  -14 -40 -12     О2   0 -58   8
        Б   14 -42  14     О3  30 -54  26     К2  46 -36  38
    letter opacity = 1 - clamp01((p1 - 0.6) / 0.3)
        — fade only near the end: half-transparent yellow over blue turns green
    island group: translate3d(0, -185 * p3 vh, 0) scale(1 - 0.06 * p3)

Section height is exactly what the scene needs to leave (162vh). Any leftover tail
shows up as a full screen of empty sky between section one and two.

── REVIEWS ──
IntersectionObserver, rootMargin '0px 0px -18% 0px', unobserve after the first hit.
    from: opacity 0; transform translateY(calc(70 * var(--u))) scale(.92) rotate(var(--tilt))
    to:   opacity 1; transform translateY(0) scale(1) rotate(var(--tilt))
    transition: opacity .45s ease, transform .78s cubic-bezier(.18, 1.5, .42, 1)
The overshoot in that curve is the bounce — do not add a separate keyframe animation.

── BOX OFFICE ──
Parallax against the distance from the viewport centre:
    strip translateY(distance * 0.03)   icons * 0.16   numbers * 0.07
Numbers count up from zero over 1100 ms, eased 1 - (1-t)^3, formatted with
toLocaleString('ru-RU'), triggered when |distance| < viewportH * 0.55.

── CINEMA ──
    q       = clamp01((position - wrapTop) / (wrapHeight - viewportH))
    zoomIn  = clamp01(q / 0.26)
    slide   = clamp01((q - 0.3) / 0.42)
    zoomOut = clamp01((q - 0.78) / 0.22)
    scale   = 2.6 - 1.6 * easeInOut(zoomIn) + 1.7 * easeInOut(zoomOut)
    visible = clamp01(zoomIn * 3) * (1 - zoomOut * 1.4)   → opacity of hall and screen
    index   = Math.min(2, Math.floor(slide * 3))
Write the track transform only when the index changes. Writing it every frame fights
the CSS transition and the slides tear in half.

── FINALE ──
IntersectionObserver on the section, threshold 0.45, plays once, freezes on the last frame.
Three fallbacks, because some browsers block autoplay:
    1. autoplay in the markup — Chrome pauses a muted video off-screen and starts it
       when it scrolls in, so do not call pause() on load;
    2. the observer above;
    3. a listener on the first pointerdown / touchstart / keydown.
And a real fallback: 700 ms after the play attempt, compare currentTime with the value
before it. If it has not moved, the browser refused — switch to scroll scrubbing:
    passed = clamp01((viewportH - sectionTop) / (viewportH + sectionHeight * 0.6))
    video.currentTime = Math.min(duration - 0.05, duration * passed)
currentTime needs no permission, so the character always rolls away.

── BUDGET ODOMETER ──
5000 → 0 across the whole document, driven by scrollY / (scrollHeight - viewportH).
Reels of digits 0…9 stacked vertically, `.odo__cell { flex: none; width: .62em }`,
shift in em: translateY(-digit em). Quantise the value to steps of 50 ₽ — otherwise the
transition restarts every frame and the digits stall. Blank the leading zeros by the index
of the first significant digit. Group separator cell: width .26em.

── SCROLL RESTORATION ──
history.scrollRestoration = 'manual' plus a second scrollTo(0,0) on load — Safari restores
the position after the load event. Skip both when the URL carries an anchor.

═══════════════════════════════════════════════════════════════════════════
10. RESPONSIVE
═══════════════════════════════════════════════════════════════════════════

TABLET, max-width 1100:
    :root --u: calc(100vw / 1250)
    .hero__stage keeps its own --u: calc(100vw / 1440) — otherwise the island and the
    letters end up wider than the frame
    .sky background-size: cover — 152% does not cover a portrait frame and the flat
    fill shows as hard bands top and bottom
    Raise small type with max(13px, calc(15 * var(--u))) etc.
    Portrait only: stack the box-office facts in one centred column, strip 150vw,
    move the couple below the heading, put its cloud on top of their legs.

MOBILE, max-width 640:
    :root --u: calc(100vw / 480)
    .hero { height: 122vh }, .hero__stage keeps --u: calc(100vw / 1440)
      and gets transform: translateY(15vh)
    island in JS: scale × 1.6, offset +12vh, transform-origin 50% 26%
      — the character has to read large while the headline still spans the frame
    scene leaves faster: p1 over 0.85 * viewportH, p3 over 1.2 * viewportH
    release: position absolute, top 11vh, centred, white, no plate — it scrolls away
      with the first screen instead of staying fixed
    budget: centred above the button, bottom 84px
    booking: centred, padding 13px 28px 15px, white-space nowrap
      (a fixed element with left: 50% and no right gets only half the frame to shrink into,
       and the label breaks in two)
    about: one column, gap 9vh, padding 11vh 20px 18vh; figures position static with
      order 1 / 2 / 3; each cloud absolute on top of its figure, z-index above it
    reviews: full width, tilts down to ±1°, gap 10vh
    box office: one centred column, strip 190vw, icons 38vw, numbers 62px
    cinema: no hall at all. Render the three ratings as white cards in the style of the
      reviews block, add a "Рейтинги" label above them, drop the sticky pin, and return
      early from the cinema scroll handler. In a narrow frame the screen hole covers almost
      the whole field, the sky shows through it and the seam reads as a layout bug.
    hero section: one column, figure 58vw, text centred,
      .hero-sec .review--cloud margin-top 78px
      (the plain `.review--cloud` selector loses to `.review:nth-child(n) { margin: 0 }`)

LANDSCAPE PHONE, orientation landscape and max-height 500:
    .hero__stage transform: translateY(-7vh) scale(.5); bottom UI pinned to the edge

═══════════════════════════════════════════════════════════════════════════
11. DO NOT
═══════════════════════════════════════════════════════════════════════════
- No text-shadow. Anywhere. Ever.
- Do not clip descenders: every masked line carries
  `overflow: hidden; padding-bottom: .16em; margin-bottom: -.16em`.
- No single-word orphan lines — text-wrap: balance on every display heading.
- The only separator allowed in page copy is "/". Never "·", never "•".
- Do not translate, shorten or re-punctuate the Russian copy.
- Do not put opaque plates over the sky or the video — glass or blur only.
- Do not call getBoundingClientRect inside the animation frame.
- Do not use Cyrillic in asset file names.
- Do not add a second typeface, a colour accent outside the palette, or any library.
- Do not replace the letter images with live text — the headline is artwork.

═══════════════════════════════════════════════════════════════════════════
12. ACCEPTANCE — run this yourself before reporting done
═══════════════════════════════════════════════════════════════════════════
Open the page at 1440, 1024, 768 and 390 wide and confirm:
 1. document.documentElement.scrollWidth equals the viewport width at every size —
    no horizontal scroll anywhere.
 2. The preloader reaches 100, disappears, and the intro plays after it, not under it.
 3. No scroll position shows an empty screen: sections meet without gaps.
 4. Every image has naturalWidth > 0 — nothing 404s.
 5. The finale video paints a frame and moves.
 6. No object covers copy to the point of illegibility.
 7. The fixed bottom labels stay readable over everything that passes beneath them.
 8. The console is clean.
 9. Compare each section against https://kolobok-vdali-ot-sborov.vercel.app at the same
    width and scroll position. Sizes, positions and timings must match, not merely resemble.

Then serve it over a Range-capable server and report the local URL.