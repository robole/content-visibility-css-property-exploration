# Exploration of `content-visibility` CSS property

The [`content-visibility`](https://developer.mozilla.org/en-US/docs/Web/CSS/content-visibility) property provides control over wheter elements are rendered or not. Some rendering steps can be skipped for elements that are not yet needed by an user to improve performance. I want to explore a range of scenarios to see the true effect on performance.

## Run the examples

I used [vite](https://vitejs.dev/) to run this as a mini-website. Just install the dependency (`npm install`) and run `vite`.

## Performance auditing

I deployed this as a website to Netlify to audit the pages for a typical environment. I performed the audit using Chrome DevTools v116 in incognito mode. I used an older laptop -- a Lenovo T460. AFAIK hardware and network issues will affect results. I ran all the scenarios in this repo in a single session using the same conditions.

## Example 1 - Landing page

This is a tasteful landing page for the artist Angèle, designed/coded by Rafaela Lucas.

<img src="img/angele.jpg" alt="screenshot overview of angele landing page" style="display:block;margin:.5rem auto;"/>

It is of typical size with 5 major sections. The main navigation menu is hidden always and can be opened via a hamburger menu. There is some JavaScript to make a slideshow widget as part of the hero section.

I think it is a good candidate for ascertaining the impact of `content-visibility` on a typical webpage.

## Use case A: Applying content-visibility to a hidden section

The website navigation section (main navigation) is hidden by default. You must click on the hamburger menu to open it.

Scenarios:
- 1: Default style.
- A2: Nav main menu has `content-visibility:auto` applied to it.
- A3: Nav main menu with `content-visibility:auto` and `content-intrinsic-size: 100vw 100vh;` specified.
- A4: Nav main menu with `content-visibility:hidden` specified. Switches value to `content-visibility:visible` when opened.

| **Name** | **Scenario**                                                                                   | **Loading** | **Scripting** | **Rendering** | **Painting** | **System** | **Idle** | **Total** |
| -------- | ---------------------------------------------------------------------------------------------- | ----------- | ------------- | ------------- | ------------ | ---------- | -------- | --------- |
| 1       | Default                                                                                        | 21          | 6             | 114           | 30           | 178        | 4608     | 4957      |
| A2       | Nav main menu has `content-visibility:auto` applied to it                                      | 34          | 7             | 181           | 82           | 212        | 4479     | 4995      |
| A3       | Nav main menu with content-visibility:auto and content-intrinsic-size: 100vw 100vh; specified. | 22          | 6             | 106           | 22           | 128        | 4715     | 4999      |
| A4       | Nav main menu with `content-visibility:hidden` specified.                                      | 30          | 7             | 144           | 21           | 155        | 4752     | 5109      |

### Conclusion

Based on these scenarios, we can conclude that you can achieve a small gain in performance when you use `content-visibility:auto`  along with `content-intrinsic-size` on a hidden element like this. It is probably not worth pursuing this type of marginal gain, unless you want things really optimized. However, if you are using `display:none` to hide the menu, you can consider `content-visibility` as an alternative.

## Use Case B - Apply `content-visibility` to offscreen sections

There are three major sections that are off-screen initially. Let's apply `content-visibility:auto` to them.

```css
.videos,
.album,
.follow {
	content-visibility: auto;
}
```

Scenarios:
- 1: Default style.
- B2: Lower 3 sections have `content-visibility:auto` applied.
- B3: Lower 3 sections have `content-visibility:auto` and `contain-intrinsic-size` specified.
- B4: Lower 3 sections have `content-visibility:hidden` applied.
- B5: Lower 3 sections have `content-visibility:hidden` and `contain-intrinsic-size` specified.
- B6: Apply `content-visibility:auto` to hero section to see if a penalty is incurred. Don't repeat this!!

| **#** | **Scenario**                                                 | **Loading** | **Scripting** | **Rendering** | **Painting** | **System** | **Idle** | **Total** |
| ----- | ------------------------------------------------------------ | ----------- | ------------- | ------------- | ------------ | ---------- | -------- | --------- |
| 1    | Default                                                      | 21          | 6             | 114           | 30           | 178        | 4608     | 4957      |
| B2    | Lower 3 sections have `content-visibility:auto` applied.     | 19          | 5             | 61            | 11           | 145        | 4744     | 4985      |
| B3    | Lower 3 sections have `content-visibility:auto` and `contain-intrinsic-size` specified. | 23          | 5             | 64            | 16           | 137        | 4673     | 4918      |
| B4    | Lower 3 sections have `content-visibility:hidden` applied.   | 18          | 4             | 69            | 22           | 135        | 4748     | 4996      |
| B5    | Lower 3 sections have `content-visibility:hidden` and `contain-intrinsic-size` specified. | 22          | 5             | 100           | 40           | 145        | 4880     | 5192      |
| B6    | Apply `content-visibility:auto` to hero section to see if a penalty is incurred. Don't repeat this!! | 32          | 5             | 103           | 22           | 156        | 4542     | 4860      |

### Conclusion

There was a significant improvement in rendering and painting when `content-visibility:auto` was used, in the region of 40% (see scenario B2 in table above). When I added `contain-intrinsic-size`, performance also improved sigificatntly (see scenario B3 in table). However, it was marginally worse than scenario B2. It is not apparent when `contain-intrinsic-size` is benefical, so it is best to test both scenarios.

Applying `content-visibility:auto` to the hero section led to a slightly longer loading time down for the page. I am not sure if the penalty would be more significant under different conditions. My guess is that it is best to avoid this scenario generally.

## Example 2 - Travel blog post

This is the demo discussed in the web.dev article - [content-visibility: the new CSS property that boosts your rendering performance](https://web.dev/content-visibility). It is a travel blog that contains a set of stories, pictures, some descriptive text, and includes some random <code>iframe</code>s!

<img src="img/travel-blog-post.jpg" alt="screenshot overview of travel blog post"/>

### Scenarios explored

1. Default style.
1. With `content-visibility:auto` applied to a few sections.

### Performance audit results

The results I found were more modest than the results of the web.dev team. For the second scenario, I observed a rendering time of 117ms, they observed a rendering time of 23ms. I do not have an explanation for the variance.

| **#** | **Scenario**                                             | **Loading** | **Scripting** | **Rendering** | **Painting** | **System** | **Idle** | **Total** |
|-------|----------------------------------------------------------|-------------|---------------|---------------|--------------|------------|----------|-----------|
| 1     | Default style                                            | 36          | 4             | 237           | 277          | 548        | 8888     | 9990      |
| 2     | With `content-visibility:auto` applied to a few sections | 66          | 2             | 117           | 157          | 421        | 9459     | 10222     |
