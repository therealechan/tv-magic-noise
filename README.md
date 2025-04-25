# TV Magic Noise App

This is a Vue 3 application that displays a living room scene with a TV that shows a realistic TV static noise effect.

## What's This Project?

This project creates an effect of a TV with static noise in a living room. The TV noise fills the entire screen as a background layer, and the living room image (with a transparent TV screen area) is placed on top. This creates the illusion that you're seeing the noise through the TV screen in the image.

## Features

- Realistic TV static noise effect created with Three.js and WebGL shaders
- Includes TV scan lines, flickering, and vignette for an authentic look
- Layered design: noise in the background with the room image on top
- Responsive design that works on different screen sizes

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
