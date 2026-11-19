
<script lang="ts">
    import {
        ArrowDown,
        ArrowLeft,
        ArrowRight,
        BusFront,
        CarFront,
        Clock3,
        Footprints,
        MapPin,
        MapPinned,
        Navigation,
        TrainFront,
        Wallet
    } from '@lucide/svelte';

    const venueMaps =
        'https://www.google.com/maps/search/?api=1&query=-1.036956%2C37.059901';

    const drivingDirections =
        'https://www.google.com/maps/dir/?api=1&destination=-1.036956%2C37.059901';

    const walkingDirections =
        'https://www.google.com/maps/dir/?api=1&origin=Murram%2C%20Thika%2C%20Kiambu%2C%20Kenya&destination=-1.036956%2C37.059901&travelmode=walking';
</script>

<svelte:head>
    <title>Venue Location | The Graduation Edit</title>
    <meta
        name="description"
        content="Find The Celebration Grounds in Thika. Get the venue location, private transport directions and public transport instructions for Ivy's graduation celebration."
    />
    <meta name="theme-color" content="#FCE5EF" />
    <meta property="og:title" content="Find Your Way | The Graduation Edit" />
    <meta
        property="og:description"
        content="Everything you need to find The Celebration Grounds in Thika."
    />
</svelte:head>

<div class="location-page">
    <header class="topbar">
        <a href="/" class="brand">
            <span class="brand-symbol">✦</span>
            THE GRADUATION EDIT
        </a>

        <a href="/" class="back-link">
            <ArrowLeft size={15} />
            Invitation
        </a>
    </header>

    <main>
        <section class="hero">
            <span class="eyebrow">YOUR DESTINATION AWAITS</span>

            <h1>
                Find Your <span>Way.</span>
            </h1>

            <p class="hero-description">
                We're looking forward to celebrating with you.
                Here's everything you need to find us.
            </p>

            <div class="event-details">
                <div>
                    <span>DATE</span>
                    <strong>Friday, 27 November 2026</strong>
                </div>
                <div class="event-divider"></div>
                <div>
                    <span>TIME</span>
                    <strong>3:00 PM onwards</strong>
                </div>
            </div>
        </section>

        <section class="venue-card">
            <div class="venue-heading">
                <div class="venue-icon">
                    <MapPin size={23} />
                </div>

                <div>
                    <span class="eyebrow">YOUR DESTINATION</span>
                    <h2>The Celebration Grounds</h2>
                    <p>Thika, Kiambu County, Kenya</p>
                </div>
            </div>

            <div class="map-preview">
                <iframe
                    title="Map showing The Celebration Grounds in Thika"
                    src="https://maps.google.com/maps?q=-1.036956%2C37.059901&z=16&output=embed"
                    loading="lazy"
                    referrerpolicy="no-referrer-when-downgrade"
                    allowfullscreen
                ></iframe>
            </div>

            <a
                class="primary-button"
                href={venueMaps}
                target="_blank"
                rel="noopener noreferrer"
            >
                <MapPinned size={18} />
                Open Venue Location in Google Maps
                <ArrowRight size={17} />
            </a>

            <p class="map-caption">
                Follow the pin. The celebration awaits.
            </p>
        </section>

        <section class="transport-section">
            <div class="section-heading">
                <span class="eyebrow">PLANNING YOUR JOURNEY</span>
                <h2>Getting <span>There</span></h2>
                <p>Choose how you'll be travelling to the celebration.</p>
            </div>

            <article class="transport-card">
                <div class="transport-title">
                    <div class="transport-icon">
                        <CarFront size={23} />
                    </div>

                    <div>
                        <span class="eyebrow">OPTION ONE</span>
                        <h3>Private Transport</h3>
                    </div>
                </div>

                <p>
                    Driving yourself or coming by taxi? Open the venue
                    directly in Google Maps and follow the route.
                </p>

                <a
                    class="secondary-button"
                    href={drivingDirections}
                    target="_blank"
                    rel="noopener noreferrer"
                >
                    <Navigation size={17} />
                    Get Driving Directions
                    <ArrowRight size={16} />
                </a>
            </article>

            <article class="transport-card public-card">
                <div class="transport-title">
                    <div class="transport-icon">
                        <BusFront size={23} />
                    </div>

                    <div>
                        <span class="eyebrow">OPTION TWO</span>
                        <h3>Public Transport</h3>
                    </div>
                </div>

                <p>
                    Coming from Nairobi CBD? Here's how to get to the
                    venue by matatu.
                </p>

                <div class="fare-card">
                    <div class="fare-icon">
                        <Wallet size={19} />
                    </div>
                    <div>
                        <span>ESTIMATED MATATU FARE</span>
                        <strong>KSh 100–130</strong>
                    </div>
                    <span class="approximate">Approximate</span>
                </div>

                <div class="route-list">
                    <div class="route-item">
                        <div class="route-marker">
                            <BusFront size={17} />
                        </div>

                        <div class="route-content">
                            <span class="step-label">STEP 01 · NAIROBI CBD</span>
                            <h4>Board a Thika-bound matatu</h4>
                            <p>Choose any of these boarding points:</p>

                            <div class="boarding-points">
                                <div>
                                    <strong>Supermetro</strong>
                                    <span>Archives</span>
                                </div>
                                <div>
                                    <strong>Manchester Sacco</strong>
                                    <span>Opposite RNG Plaza</span>
                                </div>
                                <div>
                                    <strong>Lopha</strong>
                                    <span>Odeon</span>
                                </div>
                            </div>
                        </div>
                    </div>

                    <div class="route-item">
                        <div class="route-marker">
                            <MapPin size={17} />
                        </div>

                        <div class="route-content">
                            <span class="step-label">STEP 02 · THIKA</span>
                            <h4>Alight at Murram stage</h4>
                            <p>
                                When travelling towards Thika, get off at
                                Murram stage, just after Jomoko.
                            </p>
                        </div>
                    </div>

                    <div class="route-item last-route">
                        <div class="route-marker">
                            <Footprints size={17} />
                        </div>

                        <div class="route-content">
                            <span class="step-label">STEP 03 · FINAL WALK</span>
                            <h4>Walk approximately 5 minutes</h4>
                            <p>From Murram stage:</p>

                            <ol>
                                <li>Take the estate road directly opposite the stage.</li>
                                <li>Take the first right.</li>
                                <li>The Celebration Grounds will be on your first left along the road.</li>
                            </ol>
                        </div>
                    </div>
                </div>

                <a
                    class="secondary-button"
                    href={walkingDirections}
                    target="_blank"
                    rel="noopener noreferrer"
                >
                    <Footprints size={17} />
                    Walking Directions from Murram
                    <ArrowRight size={16} />
                </a>

                <p class="walking-caption">
                    <Clock3 size={14} />
                    Approximately 5 minutes on foot.
                </p>
            </article>
        </section>

        <section class="closing">
            <span class="closing-star">✦</span>
            <h2>See you there!</h2>
            <p>
                Thank you for being part of this special milestone.
                I can't wait to celebrate with you.
            </p>
            <span class="closing-signature">WITH LOVE, IVY</span>
        </section>
    </main>

    <footer>
        <span>THE GRADUATION EDIT</span>
        <span>✦</span>
        <span>2026</span>
    </footer>
</div>

<style>
    :global(*) {
        box-sizing: border-box;
    }

    :global(body) {
        margin: 0;
    }

    .location-page {
        min-height: 100vh;
        background: #FCE5EF;
        color: #432A35;
        font-family: 'DM Sans', sans-serif;
    }

    .topbar {
        max-width: 1100px;
        margin: auto;
        padding: 22px 35px;
        display: flex;
        align-items: center;
        justify-content: space-between;
    }

    .brand {
        display: flex;
        align-items: center;
        gap: 9px;
        color: #721F42;
        font-size: 10px;
        font-weight: 700;
        letter-spacing: 2px;
        text-decoration: none;
    }

    .brand-symbol {
        color: #C69B60;
        font-size: 17px;
    }

    .back-link {
        display: flex;
        align-items: center;
        gap: 6px;
        color: #721F42;
        font-size: 11px;
        text-decoration: none;
    }

    main {
        width: min(100% - 36px, 760px);
        margin: auto;
    }

    .hero {
        padding: 66px 20px 42px;
        text-align: center;
    }

    .eyebrow {
        color: #A87948;
        font-size: 9px;
        font-weight: 700;
        letter-spacing: 2px;
    }

    .hero h1 {
        margin: 12px 0;
        color: #721F42;
        font: 400 clamp(52px, 9vw, 75px)/1 'Cormorant Garamond', Georgia, serif;
    }

    .hero h1 span,
    .section-heading h2 span {
        color: #B28A56;
        font-style: italic;
    }

    .hero-description {
        max-width: 390px;
        margin: 15px auto 25px;
        color: #856F79;
        font-size: 12px;
        line-height: 1.8;
    }

    .event-details {
        display: inline-flex;
        justify-content: center;
        align-items: center;
        gap: 23px;
        padding: 15px 22px;
        border: 1px solid rgba(198, 155, 96, 0.35);
        border-radius: 12px;
        background: rgba(255, 252, 250, 0.65);
        text-align: left;
    }

    .event-details div:not(.event-divider) {
        display: flex;
        flex-direction: column;
        gap: 5px;
    }

    .event-details span {
        color: #A87948;
        font-size: 8px;
        font-weight: 700;
        letter-spacing: 1.4px;
    }

    .event-details strong {
        color: #721F42;
        font-size: 10px;
    }

    .event-divider {
        width: 1px;
        height: 30px;
        background: #E8D2DC;
    }

    .venue-card,
    .transport-card {
        padding: 27px;
        border: 1px solid rgba(198, 155, 96, 0.3);
        border-radius: 19px;
        background: #FFFCFA;
        box-shadow: 0 12px 35px rgba(80, 32, 52, 0.045);
    }

    .venue-heading,
    .transport-title {
        display: flex;
        align-items: center;
        gap: 14px;
        margin-bottom: 21px;
    }

    .venue-icon,
    .transport-icon {
        width: 48px;
        height: 48px;
        flex-shrink: 0;
        display: grid;
        place-items: center;
        border-radius: 50%;
        background: #F8E4EB;
        color: #721F42;
    }

    .venue-heading h2 {
        margin: 4px 0;
        color: #721F42;
        font: 500 27px/1.1 'Cormorant Garamond', Georgia, serif;
    }

    .venue-heading p {
        margin: 0;
        color: #8B7880;
        font-size: 11px;
    }

    .map-preview {
        height: 280px;
        overflow: hidden;
        border: 1px solid #EBDCE2;
        border-radius: 13px;
        background: #F8E4EB;
    }

    .map-preview iframe {
        width: 100%;
        height: 100%;
        border: 0;
    }

    .primary-button,
    .secondary-button {
        min-height: 48px;
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 10px;
        margin-top: 16px;
        padding: 12px 16px;
        border-radius: 9px;
        font-size: 11px;
        font-weight: 600;
        text-decoration: none;
        transition: background 0.2s ease, transform 0.2s ease;
    }

    .primary-button {
        background: #721F42;
        color: white;
    }

    .primary-button:hover {
        transform: translateY(-2px);
        background: #8B2D53;
    }

    .secondary-button {
        border: 1px solid #E8D4DC;
        background: #FFF7FA;
        color: #721F42;
    }

    .secondary-button:hover {
        background: #FCEAF0;
        transform: translateY(-1px);
    }

    .map-caption {
        margin: 12px 0 0;
        color: #A87948;
        font: italic 15px 'Cormorant Garamond', Georgia, serif;
        text-align: center;
    }

    .transport-section {
        padding-top: 70px;
    }

    .section-heading {
        margin-bottom: 24px;
        text-align: center;
    }

    .section-heading h2 {
        margin: 8px 0;
        color: #721F42;
        font: 400 43px 'Cormorant Garamond', Georgia, serif;
    }

    .section-heading p {
        color: #8B7880;
        font-size: 11px;
    }

    .transport-card {
        margin-bottom: 17px;
    }

    .transport-title {
        margin-bottom: 10px;
    }

    .transport-title h3 {
        margin: 4px 0 0;
        color: #721F42;
        font: 500 25px 'Cormorant Garamond', Georgia, serif;
    }

    .transport-card > p {
        margin: 0;
        color: #887780;
        font-size: 11px;
        line-height: 1.8;
    }

    .fare-card {
        display: flex;
        align-items: center;
        gap: 12px;
        margin-top: 20px;
        padding: 15px;
        border: 1px solid #EAD6BB;
        border-radius: 11px;
        background: #FBF5ED;
    }

    .fare-icon {
        width: 38px;
        height: 38px;
        display: grid;
        place-items: center;
        border-radius: 50%;
        background: #F2E6D6;
        color: #A87948;
    }

    .fare-card > div:nth-child(2) {
        display: flex;
        flex-direction: column;
        gap: 4px;
    }

    .fare-card > div span {
        color: #A17C51;
        font-size: 8px;
        font-weight: 700;
        letter-spacing: 1px;
    }

    .fare-card strong {
        color: #721F42;
        font: 600 22px 'Cormorant Garamond', Georgia, serif;
    }

    .approximate {
        margin-left: auto;
        color: #927F87;
        font-size: 9px;
    }

    .route-list {
        margin-top: 25px;
    }

    .route-item {
        position: relative;
        display: flex;
        gap: 13px;
        padding-bottom: 27px;
    }

    .route-item:not(.last-route)::before {
        content: '';
        position: absolute;
        top: 36px;
        bottom: 0;
        left: 17px;
        width: 1px;
        background: #E9D4DC;
    }

    .route-marker {
        z-index: 1;
        flex: 0 0 35px;
        width: 35px;
        height: 35px;
        display: grid;
        place-items: center;
        border: 1px solid #EBD8DF;
        border-radius: 50%;
        background: #FFF8FA;
        color: #8C3D5C;
    }

    .route-content {
        min-width: 0;
        flex: 1;
    }

    .step-label {
        display: block;
        margin: 2px 0 6px;
        color: #B28A56;
        font-size: 8px;
        font-weight: 700;
        letter-spacing: 1.2px;
    }

    .route-content h4 {
        margin: 0 0 6px;
        color: #721F42;
        font-size: 12px;
        font-weight: 700;
    }

    .route-content p {
        margin: 0;
        color: #887780;
        font-size: 10px;
        line-height: 1.8;
    }

    .boarding-points {
        display: grid;
        gap: 8px;
        margin-top: 12px;
    }

    .boarding-points div {
        display: flex;
        align-items: center;
        justify-content: space-between;
        flex-wrap: wrap;
        gap: 5px;
        padding: 11px 12px;
        border: 1px solid #F0E4E8;
        border-radius: 8px;
        background: #FFFAFB;
    }

    .boarding-points strong {
        color: #721F42;
        font-size: 10px;
    }

    .boarding-points span {
        color: #927F87;
        font-size: 9px;
    }

    .route-content ol {
        display: grid;
        gap: 9px;
        margin: 11px 0 0;
        padding-left: 18px;
        color: #806D76;
        font-size: 10px;
        line-height: 1.7;
    }

    .route-content ol li::marker {
        color: #B28A56;
        font-weight: 700;
    }

    .walking-caption {
        display: flex;
        justify-content: center;
        align-items: center;
        gap: 6px;
        margin: 12px 0 0;
        color: #927F87;
        font-size: 9px;
    }

    .closing {
        padding: 65px 20px 55px;
        text-align: center;
    }

    .closing-star {
        color: #C69B60;
        font-size: 23px;
    }

    .closing h2 {
        margin: 9px 0;
        color: #721F42;
        font: italic 400 42px 'Cormorant Garamond', Georgia, serif;
    }

    .closing p {
        max-width: 360px;
        margin: auto;
        color: #887780;
        font-size: 11px;
        line-height: 1.8;
    }

    .closing-signature {
        display: block;
        margin-top: 20px;
        color: #A87948;
        font-size: 9px;
        font-weight: 700;
        letter-spacing: 2px;
    }

    footer {
        display: flex;
        justify-content: center;
        align-items: center;
        gap: 12px;
        padding: 19px;
        border-top: 1px solid rgba(198, 155, 96, 0.25);
        color: #A87948;
        font-size: 8px;
        font-weight: 700;
        letter-spacing: 1.5px;
    }

    @media (max-width: 560px) {
        .topbar {
            padding: 17px 18px;
        }

        .brand {
            font-size: 8px;
            letter-spacing: 1.3px;
        }

        main {
            width: calc(100% - 24px);
        }

        .hero {
            padding: 49px 10px 31px;
        }

        .hero h1 {
            font-size: 55px;
        }

        .event-details {
            width: 100%;
            gap: 12px;
            padding: 13px 10px;
        }

        .event-details strong {
            font-size: 9px;
        }

        .venue-card,
        .transport-card {
            padding: 19px;
            border-radius: 15px;
        }

        .venue-heading h2 {
            font-size: 23px;
        }

        .map-preview {
            height: 235px;
        }

        .primary-button,
        .secondary-button {
            font-size: 10px;
            gap: 7px;
        }

        .transport-section {
            padding-top: 52px;
        }

        .section-heading h2 {
            font-size: 39px;
        }

        .approximate {
            font-size: 8px;
        }
    }

    @media (prefers-reduced-motion: reduce) {
        *,
        *::before,
        *::after {
            scroll-behavior: auto !important;
            animation-duration: 0.01ms !important;
            transition-duration: 0.01ms !important;
        }
    }
</style>
