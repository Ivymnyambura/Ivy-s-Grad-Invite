
<script lang="ts">
    import {
        ArrowLeft,
        ArrowRight,
        BusFront,
        CarFront,
        Clock3,
        Footprints,
        MapPin,
        Navigation,
        X
    } from '@lucide/svelte';

    type Variant = 'modal' | 'page';
    type TravelMode = 'choice' | 'public';

    let {
        variant = 'modal',
        onClose = () => {}
    }: {
        variant?: Variant;
        onClose?: () => void;
    } = $props();

    let selectedMode = $state<TravelMode>('choice');

    const privateDirections =
        'https://www.google.com/maps/dir/?api=1&destination=-1.036956%2C37.059901';

    const walkingDirections =
        'https://www.google.com/maps/dir/?api=1&origin=Murram%2C%20Thika%2C%20Kiambu%2C%20Kenya&destination=-1.036956%2C37.059901&travelmode=walking';
</script>

<div class:transport-overlay={variant === 'modal'} class:transport-page={variant === 'page'}>
    <section
        class="transport-card"
        class:modal-card={variant === 'modal'}
        class:standalone-card={variant === 'page'}
        role={variant === 'modal' ? 'dialog' : undefined}
        aria-modal={variant === 'modal' ? 'true' : undefined}
        aria-labelledby="transport-title"
    >
        <div class="card-top">
            <div class="brand-mark">
                <span class="brand-line"></span>
                <span>THE GRADUATION EDIT</span>
                <span class="brand-line"></span>
            </div>

            {#if variant === 'modal'}
                <button class="close-button" onclick={onClose} aria-label="Close directions">
                    <X size={20} />
                </button>
            {:else}
                <a href="/" class="back-link">
                    <ArrowLeft size={15} />
                    Back to invitation
                </a>
            {/if}
        </div>

        <div class="card-content">
            <div class="heading">
                <span class="eyebrow">WE CAN'T WAIT TO SEE YOU</span>
                <h1 id="transport-title">
                    Getting <span>Here</span>
                </h1>
                <p class="intro">
                    On your way? A little help getting to the celebration.
                </p>
            </div>

            <div class="gold-divider">
                <span></span>
                <span class="divider-diamond">✦</span>
                <span></span>
            </div>

            {#if selectedMode === 'choice'}
                <div class="choice-section">
                    <h2>How will you be travelling?</h2>
                    <p class="section-description">
                        Choose your travel option for directions to the venue.
                    </p>

                    <div class="travel-options">
                        <a
                            class="travel-option"
                            href={privateDirections}
                            target="_blank"
                            rel="noopener noreferrer"
                        >
                            <div class="option-icon">
                                <CarFront size={25} strokeWidth={1.6} />
                            </div>

                            <div class="option-copy">
                                <h3>Private transport</h3>
                                <p>Driving to the celebration? Get directions straight to the venue.</p>
                                <span class="option-action">
                                    Open Google Maps <ArrowRight size={15} />
                                </span>
                            </div>

                            <ArrowRight class="option-arrow" size={19} />
                        </a>

                        <button
                            class="travel-option"
                            onclick={() => (selectedMode = 'public')}
                        >
                            <div class="option-icon">
                                <BusFront size={25} strokeWidth={1.6} />
                            </div>

                            <div class="option-copy">
                                <h3>Public transport</h3>
                                <p>Coming by matatu from Nairobi CBD? We've got you.</p>
                                <span class="option-action">
                                    View travel guide <ArrowRight size={15} />
                                </span>
                            </div>

                            <ArrowRight class="option-arrow" size={19} />
                        </button>
                    </div>
                </div>
            {:else}
                <div class="public-guide">
                    <button class="back-button" onclick={() => (selectedMode = 'choice')}>
                        <ArrowLeft size={16} />
                        Back to travel options
                    </button>

                    <div class="guide-heading">
                        <div class="option-icon large">
                            <BusFront size={25} strokeWidth={1.6} />
                        </div>
                        <div>
                            <span class="eyebrow">COMING FROM NAIROBI CBD</span>
                            <h2>Take a matatu to Thika</h2>
                        </div>
                    </div>

                    <div class="fare-card">
                        <div>
                            <span>ESTIMATED MATATU FARE</span>
                            <strong>KSh 100–130</strong>
                        </div>
                        <span class="fare-note">Approximate fare</span>
                    </div>

                    <div class="route-steps">
                        <div class="route-step">
                            <div class="step-icon">
                                <MapPin size={19} />
                            </div>
                            <div>
                                <span class="step-label">STEP 01 · NAIROBI CBD</span>
                                <h3>Board a Thika-bound matatu</h3>
                                <p>You can board from any of these three options:</p>
                                <ul class="pickup-list">
                                    <li><strong>Supermetro</strong><span>Archives</span></li>
                                    <li><strong>Manchester Sacco</strong><span>Opposite RNG Plaza</span></li>
                                    <li><strong>Lopha</strong><span>Odeon</span></li>
                                </ul>
                            </div>
                        </div>

                        <div class="route-step">
                            <div class="step-icon">
                                <MapPin size={19} />
                            </div>
                            <div>
                                <span class="step-label">STEP 02 · YOUR STOP</span>
                                <h3>Alight at Murram stage</h3>
                                <p>
                                    Get off at Murram stage, just after Jomoko
                                    when travelling towards Thika.
                                </p>
                            </div>
                        </div>

                        <div class="route-step last-step">
                            <div class="step-icon">
                                <Footprints size={19} />
                            </div>
                            <div>
                                <span class="step-label">STEP 03 · FINAL WALK</span>
                                <h3>A short 5-minute walk</h3>
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
                        class="walking-button"
                        href={walkingDirections}
                        target="_blank"
                        rel="noopener noreferrer"
                    >
                        <Navigation size={18} />
                        Start walking directions
                        <ArrowRight size={17} />
                    </a>

                    <p class="walking-note">
                        <Clock3 size={14} />
                        Approximately 5 minutes on foot from Murram stage.
                    </p>
                </div>
            {/if}

            <div class="venue-footer">
                <MapPin size={17} />
                <div>
                    <strong>The Celebration Grounds</strong>
                    <span>Thika, Kiambu County, Kenya</span>
                </div>
            </div>
        </div>

        <div class="card-bottom">
            <span>MADE WITH LOVE</span>
            <span>✦</span>
            <span>IVY NYAMBURA · 2026</span>
        </div>
    </section>
</div>

<style>
    :global(*) {
        box-sizing: border-box;
    }

    .transport-overlay {
        position: fixed;
        inset: 0;
        z-index: 1000;
        display: flex;
        align-items: center;
        justify-content: center;
        padding: 20px;
        overflow-y: auto;
        background: rgba(47, 21, 33, 0.55);
        backdrop-filter: blur(7px);
        animation: fade-in 0.25s ease;
    }

    .transport-page {
        min-height: 100vh;
        display: flex;
        align-items: center;
        justify-content: center;
        padding: 45px 18px;
        background:
            radial-gradient(ellipse at top, #fff5f8 0%, transparent 65%),
            #FCE5EF;
    }

    .transport-card {
        position: relative;
        width: 100%;
        max-width: 590px;
        overflow: hidden;
        background: #FFFCFA;
        border: 1px solid rgba(198, 155, 96, 0.35);
        border-radius: 23px;
        box-shadow: 0 22px 75px rgba(70, 25, 43, 0.13);
        color: #432A35;
    }

    .modal-card {
        max-height: calc(100vh - 40px);
        overflow-y: auto;
        animation: rise-in 0.3s ease;
    }

    .standalone-card {
        max-width: 660px;
    }

    .card-top {
        min-height: 65px;
        display: flex;
        align-items: center;
        justify-content: center;
        position: relative;
        padding: 15px 22px;
        border-bottom: 1px solid #F1E5E7;
    }

    .brand-mark {
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 10px;
        color: #721F42;
        font-family: 'DM Sans', sans-serif;
        font-size: 9px;
        font-weight: 700;
        letter-spacing: 2px;
    }

    .brand-line {
        width: 22px;
        height: 1px;
        background: #C69B60;
    }

    .close-button {
        position: absolute;
        right: 17px;
        top: 50%;
        transform: translateY(-50%);
        width: 35px;
        height: 35px;
        display: grid;
        place-items: center;
        border: 0;
        border-radius: 50%;
        background: #FCEAF0;
        color: #721F42;
        cursor: pointer;
        transition: background 0.2s ease;
    }

    .close-button:hover {
        background: #F7D5E2;
    }

    .back-link {
        position: absolute;
        left: 19px;
        display: inline-flex;
        align-items: center;
        gap: 6px;
        color: #721F42;
        text-decoration: none;
        font: 500 11px 'DM Sans', sans-serif;
    }

    .card-content {
        padding: 30px 39px 25px;
    }

    .heading {
        text-align: center;
    }

    .eyebrow {
        color: #B28A56;
        font: 700 9px 'DM Sans', sans-serif;
        letter-spacing: 2.1px;
    }

    h1 {
        margin: 8px 0 5px;
        color: #721F42;
        font: 400 48px/1.1 'Cormorant Garamond', Georgia, serif;
    }

    h1 span {
        color: #B28A56;
        font-style: italic;
    }

    .intro {
        margin: 8px 0 0;
        color: #88727B;
        font: 400 12px/1.6 'DM Sans', sans-serif;
    }

    .gold-divider {
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 12px;
        margin: 24px auto;
    }

    .gold-divider > span:not(.divider-diamond) {
        width: 65px;
        height: 1px;
        background: #E7D3B8;
    }

    .divider-diamond {
        color: #C69B60;
        font-size: 12px;
    }

    .choice-section h2,
    .public-guide h2 {
        margin: 0;
        color: #721F42;
        font: 400 25px/1.2 'Cormorant Garamond', Georgia, serif;
    }

    .choice-section {
        text-align: center;
    }

    .section-description {
        margin: 7px 0 19px;
        color: #927F87;
        font: 400 11px/1.6 'DM Sans', sans-serif;
    }

    .travel-options {
        display: grid;
        gap: 12px;
    }

    .travel-option {
        width: 100%;
        min-height: 112px;
        display: flex;
        align-items: center;
        gap: 15px;
        padding: 17px;
        border: 1px solid #F0DCE3;
        border-radius: 15px;
        background: #FFF8FA;
        color: inherit;
        text-align: left;
        text-decoration: none;
        cursor: pointer;
        transition: transform 0.2s ease, border-color 0.2s ease, background 0.2s ease;
    }

    .travel-option:hover {
        transform: translateY(-2px);
        border-color: #C69B60;
        background: #FFF3F6;
    }

    .option-icon {
        flex: 0 0 48px;
        width: 48px;
        height: 48px;
        display: grid;
        place-items: center;
        border-radius: 50%;
        background: #F8E4EB;
        color: #721F42;
    }

    .option-copy {
        min-width: 0;
        flex: 1;
    }

    .option-copy h3 {
        margin: 0 0 4px;
        color: #721F42;
        font: 600 12px 'DM Sans', sans-serif;
    }

    .option-copy p {
        margin: 0;
        color: #8B7880;
        font: 400 10px/1.6 'DM Sans', sans-serif;
    }

    .option-action {
        display: inline-flex;
        align-items: center;
        gap: 6px;
        margin-top: 9px;
        color: #A87948;
        font: 700 9px 'DM Sans', sans-serif;
        letter-spacing: 0.3px;
    }

    :global(.option-arrow) {
    flex: 0 0 auto;
    color: #B28A56;
}

    .public-guide {
        text-align: left;
    }

    .back-button {
        display: inline-flex;
        align-items: center;
        gap: 7px;
        margin: 0 0 22px;
        padding: 0;
        border: 0;
        background: transparent;
        color: #9A687B;
        font: 500 10px 'DM Sans', sans-serif;
        cursor: pointer;
    }

    .guide-heading {
        display: flex;
        align-items: center;
        gap: 13px;
        margin-bottom: 19px;
    }

    .option-icon.large {
        width: 49px;
        height: 49px;
        flex-basis: 49px;
    }

    .guide-heading h2 {
        margin-top: 4px;
        font-size: 23px;
    }

    .fare-card {
        display: flex;
        justify-content: space-between;
        align-items: center;
        gap: 12px;
        padding: 14px 17px;
        margin-bottom: 22px;
        border: 1px solid #EAD6BB;
        border-radius: 12px;
        background: #FBF5ED;
    }

    .fare-card > div {
        display: flex;
        flex-direction: column;
        gap: 3px;
    }

    .fare-card > div span {
        color: #A17C51;
        font: 700 8px 'DM Sans', sans-serif;
        letter-spacing: 1px;
    }

    .fare-card strong {
        color: #721F42;
        font: 600 21px 'Cormorant Garamond', Georgia, serif;
    }

    .fare-note {
        color: #927F87;
        font: 400 9px 'DM Sans', sans-serif;
    }

    .route-steps {
        position: relative;
    }

    .route-step {
        position: relative;
        display: flex;
        gap: 13px;
        padding-bottom: 23px;
    }

    .route-step:not(.last-step)::before {
        content: '';
        position: absolute;
        top: 36px;
        bottom: 0;
        left: 17px;
        width: 1px;
        background: #E9D4DC;
    }

    .step-icon {
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

    .route-step > div:last-child {
        flex: 1;
        min-width: 0;
    }

    .step-label {
        display: block;
        margin: 2px 0 5px;
        color: #B28A56;
        font: 700 8px 'DM Sans', sans-serif;
        letter-spacing: 1.2px;
    }

    .route-step h3 {
        margin: 0 0 5px;
        color: #721F42;
        font: 600 13px 'DM Sans', sans-serif;
    }

    .route-step p {
        margin: 0;
        color: #887780;
        font: 400 10px/1.7 'DM Sans', sans-serif;
    }

    .pickup-list {
        display: grid;
        gap: 7px;
        margin: 11px 0 0;
        padding: 0;
        list-style: none;
    }

    .pickup-list li {
        display: flex;
        flex-wrap: wrap;
        justify-content: space-between;
        gap: 4px 12px;
        padding: 9px 11px;
        border: 1px solid #F0E4E8;
        border-radius: 8px;
        background: #FFFAFB;
        font: 400 10px 'DM Sans', sans-serif;
    }

    .pickup-list strong {
        color: #721F42;
        font-weight: 600;
    }

    .pickup-list span {
        color: #927F87;
    }

    .route-step ol {
        display: grid;
        gap: 7px;
        margin: 9px 0 0;
        padding-left: 17px;
        color: #806D76;
        font: 400 10px/1.6 'DM Sans', sans-serif;
    }

    .route-step ol li::marker {
        color: #B28A56;
        font-weight: 700;
    }

    .walking-button {
        min-height: 48px;
        display: flex;
        justify-content: center;
        align-items: center;
        gap: 10px;
        padding: 12px 16px;
        border: 0;
        border-radius: 10px;
        background: #721F42;
        color: #FFF9FA;
        text-decoration: none;
        font: 600 11px 'DM Sans', sans-serif;
        transition: background 0.2s ease, transform 0.2s ease;
    }

    .walking-button:hover {
        transform: translateY(-1px);
        background: #8B2D53;
    }

    .walking-note {
        display: flex;
        justify-content: center;
        align-items: center;
        gap: 6px;
        margin: 10px 0 0;
        color: #9A8790;
        font: 400 9px 'DM Sans', sans-serif;
    }

    .venue-footer {
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 10px;
        margin-top: 26px;
        padding: 15px 10px;
        border-top: 1px solid #F0E4E8;
        color: #A87948;
    }

    .venue-footer div {
        display: flex;
        flex-direction: column;
        gap: 3px;
    }

    .venue-footer strong {
        color: #721F42;
        font: 600 11px 'DM Sans', sans-serif;
    }

    .venue-footer span {
        color: #98848D;
        font: 400 9px 'DM Sans', sans-serif;
    }

    .card-bottom {
        display: flex;
        justify-content: center;
        align-items: center;
        gap: 12px;
        padding: 12px;
        border-top: 1px solid #F1E5E7;
        color: #B28A56;
        font: 700 8px 'DM Sans', sans-serif;
        letter-spacing: 1.2px;
    }

    @keyframes fade-in {
        from { opacity: 0; }
        to { opacity: 1; }
    }

    @keyframes rise-in {
        from { opacity: 0; transform: translateY(12px); }
        to { opacity: 1; transform: translateY(0); }
    }

    @media (max-width: 560px) {
        .transport-overlay {
            align-items: center;
            padding: 10px;
        }

        .modal-card {
            max-height: calc(100dvh - 20px);
        }

        .transport-page {
            padding: 18px 10px;
        }

        .card-content {
            padding: 25px 20px 20px;
        }

        .card-top {
            min-height: 58px;
        }

        .brand-mark {
            font-size: 8px;
            letter-spacing: 1.5px;
        }

        .brand-line {
            width: 14px;
        }

        h1 {
            font-size: 42px;
        }

        .choice-section h2 {
            font-size: 23px;
        }

        .travel-option {
            min-height: 105px;
            gap: 11px;
            padding: 13px;
        }

        .option-icon {
            flex-basis: 42px;
            width: 42px;
            height: 42px;
        }

        .option-copy p {
            font-size: 9px;
        }

        .back-link {
            left: 12px;
            font-size: 9px;
        }

        .fare-card {
            align-items: flex-start;
            flex-direction: column;
            gap: 4px;
        }
    }

    @media (prefers-reduced-motion: reduce) {
        *,
        *::before,
        *::after {
            animation-duration: 0.01ms !important;
            transition-duration: 0.01ms !important;
        }
    }
</style>
