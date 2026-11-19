
<script lang="ts">
    import {
        ArrowLeft,
        ArrowRight,
        CalendarDays,
        Check,
        Clock3,
        Heart,
        MapPin,
        Navigation,
        Sparkles,
        Download,
        Ticket,
        Mail
    } from '@lucide/svelte';

    type Page = 'envelope' | 'invitation' | 'rsvp' | 'ticket' | 'decline';

    let page = $state<Page>('envelope');
    let guestName = $state('');
    let attendance = $state('yes');
    let guestNote = $state('');
    let submitting = $state(false);
    let errorMessage = $state('');
    let previewMode = $state(false);
    let transitioning = $state(false);

    // Add your Formspree endpoint here when it is ready.
    const FORMSPREE_ENDPOINT = '';

    const eventDate = 'Friday, 27 November 2026';
    const eventTime = '3:00 PM onwards';
    const mapLink = 'https://www.google.com/maps/dir/?api=1&destination=-1.036956%2C37.059901';

    const firstName = $derived(
        guestName.trim().split(/\s+/)[0] || 'there'
    );

    function navigateTo(nextPage: Page) {
        if (transitioning || page === nextPage) return;

        transitioning = true;

        window.setTimeout(() => {
            page = nextPage;
            window.setTimeout(() => {
                transitioning = false;
            }, 120);
        }, 550);
    }

    function goBack() {
        if (page === 'invitation') navigateTo('envelope');
        else if (page === 'rsvp') navigateTo('invitation');
        else if (page === 'ticket' || page === 'decline') navigateTo('rsvp');
    }

    function openInvitation() {
        navigateTo('invitation');
    }

    function continueToRsvp() {
        navigateTo('rsvp');
    }

    async function handleRsvp(event: SubmitEvent) {
        event.preventDefault();
        errorMessage = '';

        if (!guestName.trim()) {
            errorMessage = 'Please enter your name before continuing.';
            return;
        }

        submitting = true;

        if (!FORMSPREE_ENDPOINT) {
            previewMode = true;
            submitting = false;
            navigateTo(attendance === 'yes' ? 'ticket' : 'decline');
            return;
        }

        try {
            const formData = new FormData();
            formData.append('name', guestName.trim());
            formData.append('attendance', attendance);
            formData.append('note', guestNote);
            formData.append('event', 'The Graduation Edit — 27 November 2026');

            const response = await fetch(FORMSPREE_ENDPOINT, {
                method: 'POST',
                body: formData,
                headers: {
                    Accept: 'application/json'
                }
            });

            if (!response.ok) {
                throw new Error('Your RSVP could not be sent. Please try again.');
            }

            previewMode = false;
            navigateTo(attendance === 'yes' ? 'ticket' : 'decline');
        } catch (error) {
            errorMessage =
                error instanceof Error
                    ? error.message
                    : 'Something went wrong. Please try again.';
        } finally {
            submitting = false;
        }
    }

    function printTicket() {
        window.print();
    }
</script>

<svelte:head>
    <title>The Graduation Edit | Ivy Nyambura</title>
    <meta
        name="description"
        content="An invitation to celebrate Ivy Nyambura's graduation on Friday, 27 November 2026."
    />
    <meta name="theme-color" content="#F7BDD3" />
    <link rel="preconnect" href="https://fonts.googleapis.com" />
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin="anonymous" />
    <link
        href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:wght@400;500;600;700&family=DM+Sans:wght@400;500;600;700&display=swap"
        rel="stylesheet"
    />
</svelte:head>

<main class="site-shell">
    <div class="background-glow glow-one"></div>
    <div class="background-glow glow-two"></div>

    <header class="site-header">
        <a class="wordmark" href="#home" onclick={(event) => {
            event.preventDefault();
            navigateTo('envelope');
        }}>
            <span class="wordmark-icon"><Sparkles size={17} strokeWidth={1.5} /></span>
            <span>The Graduation Edit</span>
        </a>
        <span class="header-date">27.11.2026</span>
    </header>

    <section class="screen-stage">
        {#if page === 'envelope'}
            <div class="screen envelope-screen" id="home">
                <div class="envelope-content">
                    <div class="envelope-art" aria-hidden="true">
                        <div class="envelope-shadow"></div>
                        <div class="envelope-card">
                            <div class="card-top-line"></div>
                            <Sparkles class="envelope-sparkle" size={26} strokeWidth={1.2} />
                            <p class="envelope-card-title">A special invitation</p>
                            <div class="envelope-card-rule"></div>
                            <p class="envelope-card-name">Ivy Nyambura</p>
                        </div>
                        <div class="envelope-body">
                            <div class="envelope-flap"></div>
                            <div class="envelope-front">
                                <div class="envelope-seal">
                                    <Heart size={23} strokeWidth={1.4} />
                                </div>
                            </div>
                        </div>
                    </div>

                    <div class="envelope-copy">
                        <h1>Something wonderful<br />is worth celebrating.</h1>
                        <p>
                            A little note, a special occasion, and a day
                            I would love to share with you.
                        </p>
                        <button class="primary-button open-button" onclick={openInvitation}>
                            Open your invitation
                            <ArrowRight size={19} />
                        </button>
                    </div>
                </div>
                <p class="bottom-note">Made with love, for the people who matter.</p>
            </div>
        {:else if page === 'invitation'}
            <div class="screen invitation-screen">
                <button class="back-button" onclick={goBack}>
                    <ArrowLeft size={19} />
                    <span>Back</span>
                </button>

                <div class="invitation-content">
                    <div class="invitation-decoration">
                        <span></span>
                        <Sparkles size={22} strokeWidth={1.3} />
                        <span></span>
                    </div>

                    <h1>I'd love for you<br />to be there.</h1>
                    <p class="invitation-intro">
                        Some moments deserve to be celebrated with the
                        people who made the journey special.
                    </p>

                    <div class="graduate-name">Ivy Nyambura</div>
                    <p class="graduate-caption">is graduating!</p>

                    <div class="event-details">
                        <div class="detail-item">
                            <div class="detail-icon"><CalendarDays size={22} /></div>
                            <div>
                                <span class="detail-label">THE DATE</span>
                                <strong>Friday, 27 November 2026</strong>
                            </div>
                        </div>
                        <div class="detail-item">
                            <div class="detail-icon"><Clock3 size={22} /></div>
                            <div>
                                <span class="detail-label">THE TIME</span>
                                <strong>3:00 PM onwards</strong>
                            </div>
                        </div>
                        <div class="detail-item">
                            <div class="detail-icon"><MapPin size={22} /></div>
                            <div>
                                <span class="detail-label">THE PLACE</span>
                                <strong>Grounds</strong>
                                <a class="map-link" href={mapLink} target="_blank" rel="noreferrer">
                                    Get directions <Navigation size={14} />
                                </a>
                            </div>
                        </div>
                    </div>

                    <div class="invitation-divider">
                        <span></span>
                        <Sparkles size={15} />
                        <span></span>
                    </div>

                    <p class="invitation-closing">
                        Your presence would make the day even more special.
                    </p>

                    <button class="primary-button" onclick={continueToRsvp}>
                        Respond to invitation
                        <ArrowRight size={19} />
                    </button>
                </div>
            </div>
        {:else if page === 'rsvp'}
            <div class="screen rsvp-screen">
                <button class="back-button" onclick={goBack}>
                    <ArrowLeft size={19} />
                    <span>Back</span>
                </button>

                <div class="rsvp-content">
                    <div class="section-icon"><Mail size={25} strokeWidth={1.4} /></div>
                    <h1>Will you<br />be joining me?</h1>
                    <p class="rsvp-intro">
                        Let me know if I should save you a seat.
                    </p>

                    <form class="rsvp-form" onsubmit={handleRsvp}>
                        <label for="guest-name">Your name</label>
                        <input
                            id="guest-name"
                            type="text"
                            placeholder="Enter your full name"
                            bind:value={guestName}
                            autocomplete="name"
                            required
                        />

                        <fieldset>
                            <legend>Your response</legend>

                            <label class:choice-selected={attendance === 'yes'} class="choice-card">
                                <input type="radio" name="attendance" value="yes" bind:group={attendance} />
                                <span class="choice-radio"></span>
                                <span class="choice-copy">
                                    <strong>Absolutely! Save me a seat.</strong>
                                    <small>I wouldn't miss it.</small>
                                </span>
                                <Check class="choice-check" size={19} />
                            </label>

                            <label class:choice-selected={attendance === 'no'} class="choice-card">
                                <input type="radio" name="attendance" value="no" bind:group={attendance} />
                                <span class="choice-radio"></span>
                                <span class="choice-copy">
                                    <strong>Sending love from afar.</strong>
                                    <small>I'll be celebrating you from here.</small>
                                </span>
                                <Heart class="choice-heart" size={19} />
                            </label>
                        </fieldset>

                        <label for="guest-note">A little message <span class="optional">(optional)</span></label>
                        <textarea
                            id="guest-note"
                            placeholder="Leave Ivy a little note..."
                            bind:value={guestNote}
                            rows="3"
                        ></textarea>

                        {#if errorMessage}
                            <p class="error-message">{errorMessage}</p>
                        {/if}

                        {#if previewMode}
                            <p class="preview-message">
                                Preview mode: this response has not been submitted or saved yet.
                            </p>
                        {/if}

                        <button class="primary-button submit-button" type="submit" disabled={submitting}>
                            {#if submitting}
                                Preparing your response...
                            {:else}
                                Send my response
                                <ArrowRight size={19} />
                            {/if}
                        </button>
                    </form>
                </div>
            </div>
        {:else if page === 'ticket'}
            <div class="screen ticket-screen">
                <button class="back-button" onclick={goBack}>
                    <ArrowLeft size={19} />
                    <span>Back</span>
                </button>

                <div class="ticket-content">
                    <div class="success-icon"><Check size={27} strokeWidth={1.8} /></div>
                    <h1>It's a date, {firstName}!</h1>
                    <p class="ticket-intro">
                        I'm so happy you'll be part of this celebration.
                    </p>

                    <article class="golden-ticket">
                        <div class="ticket-top">
                            <div class="ticket-brand">
                                <Sparkles size={18} />
                                <span>THE GRADUATION EDIT</span>
                            </div>
                            <span class="ticket-number">ADMIT ONE</span>
                        </div>

                        <div class="ticket-main">
                            <p class="ticket-kicker">A DAY TO REMEMBER</p>
                            <h2>Golden<br />Ticket</h2>
                            <div class="ticket-divider"></div>
                            <p class="ticket-holder-label">PREPARED FOR</p>
                            <p class="ticket-holder">{guestName.trim()}</p>
                            <p class="ticket-message">
                                Your presence is the best gift.
                                Come ready to celebrate!
                            </p>
                        </div>

                        <div class="ticket-details">
                            <div>
                                <span>DATE</span>
                                <strong>27 NOV 2026</strong>
                            </div>
                            <div>
                                <span>TIME</span>
                                <strong>3:00 PM</strong>
                            </div>
                            <div>
                                <span>VENUE</span>
                                <strong>CELEBRATION GROUNDS</strong>
                            </div>
                        </div>

                        <div class="ticket-bottom">
                            <span>IVY NYAMBURA</span>
                            <span class="ticket-admission">ONE SPECIAL GUEST</span>
                        </div>
                    </article>

                    <p class="ticket-footnote">
                        Keep this little reminder close. I can't wait to see you!
                    </p>

                    <button class="primary-button print-button" onclick={printTicket}>
                        <Download size={18} />
                        Save or print your ticket
                    </button>
                </div>
            </div>
        {:else if page === 'decline'}
            <div class="screen decline-screen">
                <button class="back-button" onclick={goBack}>
                    <ArrowLeft size={19} />
                    <span>Back</span>
                </button>

                <div class="decline-content">
                    <div class="decline-icon"><Heart size={31} strokeWidth={1.3} /></div>
                    <h1>Sending love<br />right back to you.</h1>
                    <p class="decline-name">Dear {firstName},</p>
                    <p class="decline-message">
                        I'll miss having you there, but I completely understand.
                        Thank you for being part of my journey and for celebrating
                        this milestone with me in your own way.
                    </p>
                    <div class="decline-divider">
                        <Sparkles size={20} strokeWidth={1.3} />
                    </div>
                    <p class="decline-closing">
                        You'll be in my thoughts on the day.
                        Here's to many more moments worth celebrating.
                    </p>
                    <p class="signature">With love, Ivy</p>
                    <button class="secondary-button" onclick={() => navigateTo('envelope')}>
                        Back to the beginning
                        <ArrowRight size={18} />
                    </button>
                </div>
            </div>
        {/if}
    </section>

    <footer class="site-footer">
        <span>THE GRADUATION EDIT</span>
        <span>27 NOVEMBER 2026</span>
    </footer>

    {#if transitioning}
        <div class="transition-overlay" aria-live="polite">
            <div class="transition-mark">
                <Sparkles size={27} strokeWidth={1.3} />
            </div>
            <p>THE GRADUATION EDIT</p>
            <div class="transition-progress"><span></span></div>
        </div>
    {/if}
</main>

<style>
    :global(*) {
        box-sizing: border-box;
    }

    :global(html) {
        min-height: 100%;
        background: #F7BDD3;
    }

    :global(body) {
        margin: 0;
        min-width: 320px;
        min-height: 100vh;
        font-family: 'DM Sans', sans-serif;
        color: #721F42;
        background: #F7BDD3;
    }

    :global(button),
    :global(input),
    :global(textarea) {
        font: inherit;
    }

    :global(button:focus-visible),
    :global(a:focus-visible),
    :global(input:focus-visible),
    :global(textarea:focus-visible) {
        outline: 3px solid #A76B86;
        outline-offset: 4px;
    }

    .site-shell {
        position: relative;
        display: flex;
        flex-direction: column;
        min-height: 100svh;
        overflow: hidden;
        isolation: isolate;
        background:
            radial-gradient(ellipse at 12% 20%, rgba(255,255,255,.45), transparent 34%),
            radial-gradient(ellipse at 90% 80%, rgba(255,255,255,.35), transparent 32%),
            linear-gradient(145deg, #F7BDD3 0%, #F9D2E1 48%, #FCE5EF 100%);
    }

    .background-glow {
        position: absolute;
        z-index: -1;
        width: 340px;
        height: 340px;
        border-radius: 50%;
        filter: blur(80px);
        opacity: .25;
        pointer-events: none;
    }

    .glow-one {
        top: 8%;
        left: -190px;
        background: #fff;
    }

    .glow-two {
        right: -190px;
        bottom: 5%;
        background: #C69B60;
        opacity: .13;
    }

    .site-header {
        width: 100%;
        max-width: 1240px;
        margin: 0 auto;
        padding: 25px 6%;
        display: flex;
        justify-content: space-between;
        align-items: center;
        border-bottom: 1px solid rgba(114,31,66,.12);
    }

    .wordmark {
        display: inline-flex;
        align-items: center;
        gap: 9px;
        color: #721F42;
        text-decoration: none;
        font-family: 'Cormorant Garamond', serif;
        font-size: 21px;
        font-weight: 600;
        letter-spacing: .01em;
    }

    .wordmark-icon {
        display: inline-flex;
        align-items: center;
        justify-content: center;
        color: #A87948;
    }

    .header-date {
        font-size: 11px;
        font-weight: 700;
        letter-spacing: .14em;
        color: rgba(114,31,66,.7);
    }

    .screen-stage {
        width: 100%;
        flex: 1;
        display: flex;
        align-items: stretch;
        justify-content: center;
    }

    .screen {
        position: relative;
        width: 100%;
        max-width: 1050px;
        min-height: 650px;
        margin: auto;
        padding: 65px 6% 75px;
        display: flex;
        align-items: center;
        justify-content: center;
        animation: screen-enter .7s cubic-bezier(.2,.7,.2,1) both;
    }

    @keyframes screen-enter {
        from {
            opacity: 0;
            transform: translateY(13px);
        }
        to {
            opacity: 1;
            transform: translateY(0);
        }
    }

    h1,
    h2,
    p {
        margin-top: 0;
    }

    h1 {
        margin-bottom: 18px;
        font-family: 'Cormorant Garamond', serif;
        font-size: clamp(48px, 6vw, 76px);
        line-height: .99;
        font-weight: 500;
        letter-spacing: -.035em;
        color: #721F42;
    }

    .site-footer {
        width: 100%;
        max-width: 1240px;
        margin: 0 auto;
        padding: 18px 6% 22px;
        display: flex;
        justify-content: space-between;
        gap: 15px;
        border-top: 1px solid rgba(114,31,66,.12);
        color: rgba(114,31,66,.65);
        font-size: 10px;
        font-weight: 700;
        letter-spacing: .13em;
    }

    .primary-button,
    .secondary-button {
        min-height: 58px;
        padding: 0 27px;
        border-radius: 999px;
        display: inline-flex;
        align-items: center;
        justify-content: center;
        gap: 14px;
        border: 1px solid #721F42;
        background: #721F42;
        color: #FFF8FA;
        font-size: 14px;
        font-weight: 600;
        cursor: pointer;
        text-decoration: none;
        transition: transform .2s ease, box-shadow .2s ease, background .2s ease;
    }

    .primary-button:hover,
    .secondary-button:hover {
        transform: translateY(-2px);
        box-shadow: 0 9px 22px rgba(114,31,66,.17);
    }

    .primary-button:disabled {
        opacity: .65;
        cursor: wait;
        transform: none;
    }

    .back-button {
        position: absolute;
        top: 30px;
        left: 6%;
        min-width: 112px;
        min-height: 46px;
        padding: 0 18px;
        display: inline-flex;
        align-items: center;
        justify-content: center;
        gap: 10px;
        border: 1px solid rgba(114,31,66,.38);
        border-radius: 999px;
        background: rgba(255,248,250,.58);
        color: #721F42;
        font-size: 13px;
        font-weight: 700;
        letter-spacing: .06em;
        text-transform: uppercase;
        cursor: pointer;
        transition: background .2s ease, transform .2s ease;
    }

    .back-button:hover {
        background: #FFF8FA;
        transform: translateX(-3px);
    }

    /* Envelope */

    .envelope-screen {
        min-height: 690px;
        flex-direction: column;
        text-align: center;
    }

    .envelope-content {
        width: 100%;
        display: flex;
        align-items: center;
        justify-content: center;
        gap: clamp(40px, 8vw, 110px);
    }

    .envelope-art {
        position: relative;
        width: 310px;
        height: 260px;
        flex: 0 0 310px;
        perspective: 900px;
    }

    .envelope-shadow {
        position: absolute;
        width: 240px;
        height: 35px;
        left: 35px;
        bottom: 0;
        border-radius: 50%;
        background: rgba(114,31,66,.15);
        filter: blur(15px);
    }

    .envelope-card {
        position: absolute;
        z-index: 1;
        width: 190px;
        height: 205px;
        top: 4px;
        left: 60px;
        padding: 25px 15px;
        display: flex;
        flex-direction: column;
        align-items: center;
        background: #FFF8FA;
        border: 1px solid rgba(198,155,96,.55);
        box-shadow: 0 12px 30px rgba(114,31,66,.1);
        transform: rotate(-3deg);
        animation: card-float 4s ease-in-out infinite;
    }

    @keyframes card-float {
        0%, 100% { transform: translateY(0) rotate(-3deg); }
        50% { transform: translateY(-8px) rotate(-1deg); }
    }

    .card-top-line {
        width: 100%;
        height: 3px;
        background: #C69B60;
    }

    .envelope-sparkle {
        margin-top: 19px;
        color: #C69B60;
    }

    .envelope-card-title {
        margin: 13px 0 8px;
        font-family: 'Cormorant Garamond', serif;
        font-size: 21px;
        font-style: italic;
    }

    .envelope-card-rule {
        width: 45px;
        height: 1px;
        background: #C69B60;
    }

    .envelope-card-name {
        margin: 11px 0 0;
        font-family: 'Cormorant Garamond', serif;
        font-size: 19px;
    }

    .envelope-body {
        position: absolute;
        z-index: 2;
        width: 280px;
        height: 165px;
        left: 15px;
        bottom: 12px;
        background: #EFAFC8;
        border: 1px solid rgba(114,31,66,.18);
        box-shadow: 0 16px 25px rgba(114,31,66,.13);
    }

    .envelope-flap {
        position: absolute;
        inset: 0;
        background: #F6C7D9;
        clip-path: polygon(0 0, 100% 0, 50% 66%);
        border-bottom: 1px solid rgba(114,31,66,.12);
    }

    .envelope-front {
        position: absolute;
        inset: 0;
        background: #F2B9D0;
        clip-path: polygon(0 0, 50% 55%, 100% 0, 100% 100%, 0 100%);
    }

    .envelope-seal {
        position: absolute;
        left: 50%;
        top: 48%;
        width: 49px;
        height: 49px;
        transform: translate(-50%, -50%);
        display: grid;
        place-items: center;
        border-radius: 50%;
        background: #721F42;
        color: #FCE5EF;
        box-shadow: 0 3px 9px rgba(114,31,66,.2);
    }

    .envelope-copy {
        max-width: 420px;
        text-align: left;
    }

    .envelope-copy h1 {
        font-size: clamp(47px, 5.2vw, 67px);
    }

    .envelope-copy > p {
        max-width: 360px;
        margin-bottom: 28px;
        color: rgba(114,31,66,.78);
        font-size: 16px;
        line-height: 1.8;
    }

    .bottom-note {
        position: absolute;
        bottom: 19px;
        left: 0;
        width: 100%;
        margin: 0;
        color: rgba(114,31,66,.58);
        font-size: 12px;
        letter-spacing: .04em;
    }

    /* Invitation */

    .invitation-screen {
        padding-top: 95px;
    }

    .invitation-content {
        width: 100%;
        max-width: 590px;
        text-align: center;
    }

    .invitation-decoration {
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 15px;
        margin-bottom: 25px;
        color: #B58B56;
    }

    .invitation-decoration span {
        width: 48px;
        height: 1px;
        background: #C69B60;
    }

    .invitation-content h1 {
        font-size: clamp(52px, 7vw, 79px);
    }

    .invitation-intro {
        max-width: 470px;
        margin: 0 auto 28px;
        color: rgba(114,31,66,.78);
        font-size: 16px;
        line-height: 1.8;
    }

    .graduate-name {
        font-family: 'Cormorant Garamond', serif;
        font-size: clamp(34px, 4vw, 45px);
        font-weight: 600;
        color: #A87948;
    }

    .graduate-caption {
        margin: 0 0 30px;
        font-family: 'Cormorant Garamond', serif;
        font-size: 23px;
        font-style: italic;
    }

    .event-details {
        padding: 23px 28px;
        display: flex;
        flex-direction: column;
        gap: 22px;
        text-align: left;
        border: 1px solid rgba(198,155,96,.45);
        background: rgba(255,248,250,.52);
        box-shadow: 0 12px 35px rgba(114,31,66,.045);
    }

    .detail-item {
        display: flex;
        align-items: center;
        gap: 17px;
    }

    .detail-icon {
        width: 43px;
        height: 43px;
        flex: 0 0 43px;
        display: grid;
        place-items: center;
        border-radius: 50%;
        background: #F8DCE7;
        color: #A87948;
    }

    .detail-item > div:last-child {
        display: flex;
        flex-direction: column;
        gap: 4px;
    }

    .detail-label {
        color: #A87948;
        font-size: 10px;
        font-weight: 700;
        letter-spacing: .14em;
    }

    .detail-item strong {
        font-size: 15px;
        font-weight: 600;
    }

    .map-link {
        width: fit-content;
        display: inline-flex;
        align-items: center;
        gap: 5px;
        color: #8D4561;
        font-size: 12px;
        font-weight: 600;
        text-decoration: none;
    }

    .map-link:hover {
        text-decoration: underline;
    }

    .invitation-divider {
        margin: 27px 0 18px;
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 12px;
        color: #B58B56;
    }

    .invitation-divider span {
        width: 55px;
        height: 1px;
        background: rgba(198,155,96,.6);
    }

    .invitation-closing {
        margin-bottom: 22px;
        font-family: 'Cormorant Garamond', serif;
        font-size: 22px;
        font-style: italic;
    }

    /* RSVP */

    .rsvp-screen {
        padding-top: 100px;
    }

    .rsvp-content {
        width: 100%;
        max-width: 520px;
    }

    .section-icon {
        width: 53px;
        height: 53px;
        margin: 0 auto 19px;
        display: grid;
        place-items: center;
        border: 1px solid rgba(198,155,96,.55);
        border-radius: 50%;
        color: #A87948;
        background: rgba(255,248,250,.55);
    }

    .rsvp-content h1 {
        text-align: center;
        font-size: clamp(52px, 7vw, 75px);
    }

    .rsvp-intro {
        margin-bottom: 30px;
        text-align: center;
        color: rgba(114,31,66,.75);
        font-size: 16px;
    }

    .rsvp-form {
        display: flex;
        flex-direction: column;
        gap: 12px;
    }

    .rsvp-form > label,
    .rsvp-form legend {
        padding: 0;
        color: #721F42;
        font-size: 13px;
        font-weight: 700;
    }

    .optional {
        color: rgba(114,31,66,.6);
        font-weight: 400;
    }

    .rsvp-form input[type="text"],
    .rsvp-form textarea {
        width: 100%;
        padding: 15px 17px;
        border: 1px solid rgba(114,31,66,.2);
        border-radius: 12px;
        background: rgba(255,248,250,.75);
        color: #721F42;
        font-size: 15px;
        outline: none;
        resize: vertical;
        transition: border .2s ease, box-shadow .2s ease;
    }

    .rsvp-form input[type="text"]::placeholder,
    .rsvp-form textarea::placeholder {
        color: rgba(114,31,66,.46);
    }

    .rsvp-form input[type="text"]:focus,
    .rsvp-form textarea:focus {
        border-color: #A87948;
        box-shadow: 0 0 0 3px rgba(198,155,96,.13);
    }

    .rsvp-form fieldset {
        min-width: 0;
        margin: 10px 0 5px;
        padding: 0;
        display: flex;
        flex-direction: column;
        gap: 10px;
        border: 0;
    }

    .rsvp-form legend {
        margin-bottom: 11px;
    }

    .choice-card {
        position: relative;
        min-height: 76px;
        padding: 15px 17px;
        display: flex;
        align-items: center;
        gap: 13px;
        border: 1px solid rgba(114,31,66,.2);
        border-radius: 13px;
        background: rgba(255,248,250,.58);
        cursor: pointer;
        transition: border .2s ease, background .2s ease, transform .2s ease;
    }

    .choice-card:hover {
        transform: translateY(-1px);
        border-color: rgba(114,31,66,.48);
    }

    .choice-card.choice-selected {
        border-color: #721F42;
        background: rgba(255,248,250,.9);
        box-shadow: 0 4px 13px rgba(114,31,66,.06);
    }

    .choice-card input {
        position: absolute;
        width: 1px;
        height: 1px;
        opacity: 0;
    }

    .choice-radio {
        width: 19px;
        height: 19px;
        flex: 0 0 19px;
        border: 1.5px solid rgba(114,31,66,.48);
        border-radius: 50%;
    }

    .choice-selected .choice-radio {
        border: 6px solid #721F42;
    }

    .choice-copy {
        display: flex;
        flex: 1;
        flex-direction: column;
        gap: 5px;
    }

    .choice-copy strong {
        font-size: 14px;
        font-weight: 700;
    }

    .choice-copy small {
        color: rgba(114,31,66,.67);
        font-size: 12px;
    }

    .choice-check,
    .choice-heart {
        color: #A87948;
        opacity: .85;
    }

    .submit-button {
        width: 100%;
        margin-top: 9px;
    }

    .error-message,
    .preview-message {
        margin: 3px 0;
        font-size: 12px;
        line-height: 1.5;
    }

    .error-message {
        color: #A02035;
    }

    .preview-message {
        padding: 10px 12px;
        border-radius: 9px;
        background: rgba(255,248,250,.75);
        color: #721F42;
    }

    /* Golden ticket */

    .ticket-screen {
        padding-top: 100px;
    }

    .ticket-content {
        width: 100%;
        max-width: 570px;
        text-align: center;
    }

    .success-icon {
        width: 53px;
        height: 53px;
        margin: 0 auto 18px;
        display: grid;
        place-items: center;
        border-radius: 50%;
        background: #721F42;
        color: #FFF8FA;
    }

    .ticket-content h1 {
        font-size: clamp(43px, 6vw, 65px);
    }

    .ticket-intro {
        margin-bottom: 27px;
        color: rgba(114,31,66,.76);
        font-size: 15px;
        line-height: 1.7;
    }

    .golden-ticket {
        position: relative;
        overflow: hidden;
        padding: 22px 26px 20px;
        text-align: left;
        color: #5C3E22;
        background:
            radial-gradient(circle at 10% 15%, rgba(255,255,255,.8), transparent 26%),
            linear-gradient(135deg, #F4E1B9 0%, #E7C98E 48%, #F6E8C8 100%);
        border: 1px solid #C69B60;
        box-shadow: 0 16px 38px rgba(114,31,66,.15);
    }

    .golden-ticket::before,
    .golden-ticket::after {
        content: '';
        position: absolute;
        top: 52%;
        width: 25px;
        height: 25px;
        border-radius: 50%;
        background: #F7BDD3;
        transform: translateY(-50%);
    }

    .golden-ticket::before { left: -13px; }
    .golden-ticket::after { right: -13px; }

    .ticket-top,
    .ticket-bottom {
        display: flex;
        justify-content: space-between;
        align-items: center;
        gap: 12px;
    }

    .ticket-brand {
        display: flex;
        align-items: center;
        gap: 8px;
        font-size: 10px;
        font-weight: 700;
        letter-spacing: .13em;
    }

    .ticket-number {
        padding: 6px 9px;
        border: 1px solid rgba(92,62,34,.5);
        font-size: 9px;
        font-weight: 700;
        letter-spacing: .1em;
    }

    .ticket-main {
        padding: 28px 0 24px;
        text-align: center;
    }

    .ticket-kicker,
    .ticket-holder-label {
        margin-bottom: 8px;
        font-size: 9px;
        font-weight: 700;
        letter-spacing: .19em;
    }

    .ticket-main h2 {
        margin-bottom: 15px;
        font-family: 'Cormorant Garamond', serif;
        font-size: clamp(58px, 9vw, 83px);
        line-height: .82;
        font-weight: 500;
        letter-spacing: -.04em;
        color: #721F42;
    }

    .ticket-divider {
        width: 54px;
        height: 1px;
        margin: 19px auto;
        background: #A87948;
    }

    .ticket-holder-label {
        margin-bottom: 4px;
    }

    .ticket-holder {
        margin-bottom: 8px;
        font-family: 'Cormorant Garamond', serif;
        font-size: 29px;
        font-weight: 600;
        color: #721F42;
    }

    .ticket-message {
        max-width: 300px;
        margin: 0 auto;
        font-family: 'Cormorant Garamond', serif;
        font-size: 19px;
        font-style: italic;
        line-height: 1.3;
    }

    .ticket-details {
        padding: 15px 0;
        display: grid;
        grid-template-columns: 1fr 1fr 1.5fr;
        gap: 12px;
        border-top: 1px dashed rgba(92,62,34,.55);
        border-bottom: 1px dashed rgba(92,62,34,.55);
    }

    .ticket-details div {
        display: flex;
        flex-direction: column;
        gap: 6px;
    }

    .ticket-details span {
        font-size: 8px;
        font-weight: 700;
        letter-spacing: .13em;
    }

    .ticket-details strong {
        font-size: 10px;
        line-height: 1.4;
    }

    .ticket-bottom {
        padding-top: 15px;
        font-size: 9px;
        font-weight: 700;
        letter-spacing: .1em;
    }

    .ticket-admission {
        color: rgba(92,62,34,.72);
    }

    .ticket-footnote {
        margin: 19px 0;
        font-family: 'Cormorant Garamond', serif;
        font-size: 19px;
        font-style: italic;
    }

    .print-button {
        min-width: 245px;
    }

    /* Decline */

    .decline-screen {
        padding-top: 100px;
    }

    .decline-content {
        max-width: 530px;
        text-align: center;
    }

    .decline-icon {
        width: 65px;
        height: 65px;
        margin: 0 auto 22px;
        display: grid;
        place-items: center;
        border: 1px solid rgba(198,155,96,.55);
        border-radius: 50%;
        color: #A87948;
        background: rgba(255,248,250,.5);
    }

    .decline-content h1 {
        font-size: clamp(50px, 7vw, 76px);
    }

    .decline-name {
        margin: 24px 0 10px;
        font-family: 'Cormorant Garamond', serif;
        font-size: 26px;
        font-weight: 600;
    }

    .decline-message,
    .decline-closing {
        color: rgba(114,31,66,.8);
        font-size: 15px;
        line-height: 1.9;
    }

    .decline-divider {
        margin: 25px 0;
        color: #A87948;
    }

    .decline-closing {
        font-family: 'Cormorant Garamond', serif;
        font-size: 21px;
        font-style: italic;
    }

    .signature {
        margin: 20px 0 27px;
        font-family: 'Cormorant Garamond', serif;
        font-size: 29px;
        font-style: italic;
        font-weight: 600;
    }

    .secondary-button {
        background: transparent;
        color: #721F42;
    }

    .secondary-button:hover {
        background: rgba(255,248,250,.55);
    }

    /* Loading transition */

    .transition-overlay {
        position: fixed;
        z-index: 20;
        inset: 0;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        gap: 17px;
        background: rgba(252,229,239,.97);
        animation: overlay-in .25s ease both;
    }

    @keyframes overlay-in {
        from { opacity: 0; }
        to { opacity: 1; }
    }

    .transition-mark {
        width: 62px;
        height: 62px;
        display: grid;
        place-items: center;
        border: 1px solid rgba(198,155,96,.65);
        border-radius: 50%;
        color: #A87948;
        animation: mark-pulse 1.2s ease-in-out infinite;
    }

    @keyframes mark-pulse {
        0%, 100% { transform: scale(1); opacity: .7; }
        50% { transform: scale(1.08); opacity: 1; }
    }

    .transition-overlay p {
        margin: 0;
        font-size: 10px;
        font-weight: 700;
        letter-spacing: .2em;
        color: #721F42;
    }

    .transition-progress {
        width: 110px;
        height: 2px;
        overflow: hidden;
        background: rgba(114,31,66,.15);
    }

    .transition-progress span {
        display: block;
        width: 40%;
        height: 100%;
        background: #A87948;
        animation: progress-move .65s ease-in-out infinite;
    }

    @keyframes progress-move {
        from { transform: translateX(-110%); }
        to { transform: translateX(280%); }
    }

    /* Responsive layout */

    @media (max-width: 700px) {
        .site-header {
            padding: 19px 6%;
        }

        .wordmark {
            font-size: 19px;
        }

        .header-date {
            font-size: 9px;
        }

        .screen {
            min-height: 600px;
            padding: 80px 7% 65px;
        }

        .envelope-screen {
            min-height: 650px;
        }

        .envelope-content {
            flex-direction: column;
            gap: 25px;
        }

        .envelope-art {
            transform: scale(.86);
            margin: -10px 0 -15px;
        }

        .envelope-copy {
            text-align: center;
        }

        .envelope-copy h1 {
            font-size: clamp(44px, 11vw, 59px);
        }

        .envelope-copy > p {
            margin-right: auto;
            margin-left: auto;
            font-size: 15px;
        }

        .back-button {
            top: 20px;
            left: 6%;
            min-width: 105px;
            min-height: 44px;
            font-size: 12px;
        }

        .invitation-screen,
        .rsvp-screen,
        .ticket-screen,
        .decline-screen {
            padding-top: 90px;
        }

        .invitation-content h1,
        .rsvp-content h1,
        .decline-content h1 {
            font-size: clamp(47px, 12vw, 64px);
        }

        .invitation-intro {
            font-size: 15px;
        }

        .event-details {
            padding: 20px 17px;
            gap: 19px;
        }

        .detail-item strong {
            font-size: 14px;
        }

        .invitation-closing {
            font-size: 20px;
        }

        .rsvp-content {
            max-width: 470px;
        }

        .ticket-details {
            grid-template-columns: 1fr 1fr;
        }

        .ticket-details div:last-child {
            grid-column: 1 / -1;
        }

        .site-footer {
            padding: 16px 6%;
            font-size: 8px;
        }
    }

    @media (max-width: 390px) {
        .wordmark {
            font-size: 17px;
        }

        .header-date {
            font-size: 8px;
        }

        .screen {
            padding-right: 6%;
            padding-left: 6%;
        }

        .envelope-art {
            transform: scale(.76);
            margin-top: -25px;
            margin-bottom: -30px;
        }

        .choice-card {
            padding: 13px 12px;
            gap: 10px;
        }

        .choice-copy strong {
            font-size: 13px;
        }

        .choice-copy small {
            font-size: 11px;
        }

        .golden-ticket {
            padding-right: 18px;
            padding-left: 18px;
        }

        .ticket-brand {
            font-size: 8px;
        }

        .ticket-number {
            font-size: 8px;
        }
    }

    @media (prefers-reduced-motion: reduce) {
        *,
        *::before,
        *::after {
            scroll-behavior: auto !important;
            animation-duration: .01ms !important;
            animation-iteration-count: 1 !important;
            transition-duration: .01ms !important;
        }
    }

    @media print {
        @page {
            size: A5 portrait;
            margin: 10mm;
        }

        :global(body),
        .site-shell {
            min-height: 0;
            background: #fff !important;
        }

        .site-header,
        .site-footer,
        .back-button,
        .ticket-intro,
        .success-icon,
        .ticket-footnote,
        .print-button,
        .background-glow {
            display: none !important;
        }

        .screen-stage,
        .ticket-screen,
        .ticket-content {
            display: block;
            min-height: 0;
            padding: 0;
            margin: 0;
        }

        .ticket-content h1 {
            margin: 0 0 15px;
            font-size: 30px;
        }

        .golden-ticket {
            width: 100%;
            box-shadow: none;
            print-color-adjust: exact;
            -webkit-print-color-adjust: exact;
        }
    }
</style>
