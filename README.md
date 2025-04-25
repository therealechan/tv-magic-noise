# TV Magic Noise App

This is a Vue 3 application that displays a living room scene with a TV that shows a realistic TV static noise effect.

## What's This Project?

This project creates an effect of a TV with static noise in a living room. The TV noise fills the entire screen as a background layer, and the living room image (with a transparent TV screen area) is placed on top. This creates the illusion that you're seeing the noise through the TV screen in the image.

## Features

- Realistic TV static noise effect created with Three.js and WebGL shaders
- Includes TV scan lines, flickering, and vignette for an authentic look
- Layered design: noise in the background with the room image on top
- Responsive design that works on different screen sizes
- Mobile-friendly with touch optimizations and performance enhancements
- SEO optimized with proper meta tags and semantic HTML

## Important Note About the Image

This app requires that your `living-room.png` has a transparent area where the TV screen should be. The TV static will show through this transparent area.

If your image doesn't have a transparent TV screen area:
1. You'll need to edit it in an image editing program (like Photoshop, GIMP, or Figma)
2. Cut out the TV screen area and make it transparent
3. Save as a PNG file to preserve the transparency

## How to Use

1. Make sure you have [Node.js](https://nodejs.org/) installed on your computer
2. Install pnpm if you don't have it already:
   ```
   npm install -g pnpm
   ```

3. Install the project dependencies:
   ```
   pnpm install
   ```

4. Start the development server:
   ```
   pnpm dev
   ```

5. Open your web browser and go to http://localhost:5173 (or the URL shown in the terminal)

## How It Works

- The app uses Vue 3, a popular framework for building web applications
- We use Three.js to create a realistic TV static noise effect with WebGL shaders
- The noise fills the entire screen as a background layer
- The living room image with a transparent TV screen is placed on top
- This creates the effect of seeing the noise through just the TV screen area

## Customizing

You can customize the TV noise effect by modifying the shader code in `EnhancedTVNoise.vue`:
- Adjust the scan line intensity
- Change the flickering frequency
- Modify the vignette effect
- Alter the color tint for different looks

## Mobile Optimization

The application has been optimized for mobile devices with:

1. **Responsive UI**: Interface elements adapt to screen size
2. **Touch-friendly Controls**: Larger tap targets for buttons on mobile
3. **Performance Optimizations**:
   - Lower resolution rendering on mobile devices
   - Simplified shaders for better performance
   - Frame rate adjustments for battery efficiency
   - Touch event optimizations to prevent unnecessary actions
4. **Mobile Display Improvements**:
   - Proper viewport configuration
   - iOS-specific fixes for full-screen display
   - Horizontal layout for controls on small screens
   - Adjusted opacity and size of UI elements
5. **Mobile Browser Compatibility**:
   - Special handling for iOS Safari
   - Android Chrome optimizations
   - PWA (Progressive Web App) capability

## SEO Implementation

This project has been optimized for search engines with:

1. **Meta Tags**: Complete set of meta tags including description, keywords, and Open Graph/Twitter Card tags
2. **Semantic HTML**: Using proper semantic elements like `<main>`, `<section>`, and `<footer>`
3. **Accessibility**: ARIA attributes to improve accessibility and SEO
4. **Structured Data**: JSON-LD implementation for rich snippets in search results
5. **Performance Optimizations**: Image loading attributes and resource priorities
6. **Robots.txt & Sitemap**: Proper crawler instructions and site structure information
7. **Production URLs**: All URLs use the production domain https://tv-magic-noise.0xechan.xyz/

## Additional SEO Considerations

To further improve your site's search engine ranking:
- Register your site with Google Search Console and Bing Webmaster Tools
- Create quality backlinks from relevant websites
- Monitor your site's performance using tools like Lighthouse
- Consider adding schema.org markup for additional content types if expanding the site
- Ensure your site loads quickly by optimizing images and assets
