# Exploration of `content-visibility` CSS property

## Example 1 - Landing page

This is a tasteful landing page for the artist Angèle, designed/coded by Rafaela
Lucas.

<img src="img/angele.png" alt="screenshot overview of angele landing page" style="max-height:500px;display:block;margin:.5rem auto;"/>

It is of typical size with 4 major sections. The main nav is hidden always and can be opened via a hamburger menu.It has some
JavaScript to make a slideshow widget as part of the hero section.

I think it is a good candidate for realistically testing what `content-visibility` can do for a typical webpage.

### Scenarios explored

I will test some scenarios to see the impact that the property can have
when applied to different parts of the page.

1. Default style. <a href="1-landing-page/1-default/">Visit demo</a>
1. Nav main menu has `content-visibility:auto` applied to it. <a href="1-landing-page/2-nav-menu-content-visbility/">Visit demo</a>
1. Nav main menu with `content-visibility:auto` and `content-intrinsic-size: 100vw 100vh;` specified. <a href="1-landing-page/3-nav-menu-content-visbility-and-sizes/"
            >Visit demo</a
          >.
1. Lower 3 sections have `content-visibility:auto` applied. <a href="1-landing-page/4-lower-sections-content-visibility/">Visit demo</a>.
1. Lower 3 sections have `content-visibility:auto` and `contain-intrinsic-size` specified. <a
            href="/1-landing-page/5-lower-sections-content-visibility-and-sizes/"
            >View demo</a
          >.
1. Lower 3 sections have `content-visibility:hidden` applied. <a href="1-landing-page/6-lower-sections-content-visibility-hidden/"
            >Visit demo</a
          >.
1. Apply `content-visibility:auto` to hero section to see if a penalty is incurred. Don't repeat this!! <a href="1-landing-page/7-hero-section-content-visibility/"
            >Visit demo</a
          >.

## Performance audit results
