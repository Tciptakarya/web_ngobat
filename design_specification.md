# UI/UX Design Specification: Ngopi Bareng Teman

This document outlines the design specifications for the "Ngopi Bareng Teman" web and mobile application, adapted to match the brand's primary yellow and black identity.

## 1. Global Styles

### Colors

* **Primary Brand/Accent Color:** Bright Yellow (approx. `#FFE600`) - Used for main CTA buttons, highlighted text, active states, and decorative graphic accents.
* **Background Color:** Clean Off-White or Very Pale Cream (approx. `#FAFAFA` or `#FFFDF5`) - Used for the main body background to keep the interface looking clean and let the yellow pop.
* **Card Background:** Pure White (`#FFFFFF`) - Used for product cards and floating category bars to maintain contrast.
* **Text Primary:** Solid Black or Dark Charcoal (approx. `#111111`) - Matches the line art of the logo. Used for headings, primary body text, and text inside primary yellow buttons.
* **Text Secondary:** Medium Gray (approx. `#666666`) - Used for subtitles and descriptions.
* **Tag Backgrounds:**
  * Best Seller: Primary Bright Yellow (`#FFE600`) with Black text.
  * New: Black (`#111111`) with White text (for contrast against the yellow).

### Typography

* **Font Family:** 
  * *Headings/Display:* A slightly playful or rounded Sans-Serif to complement the hand-drawn logo (e.g., *Quicksand*, *Nunito*, or even a legible marker font for major hero text).
  * *Body Text:* A clean, highly legible Sans-Serif (e.g., *Inter*, *Poppins*).
* **Headings:** Bold and heavy weight. The Hero title features mixed colors (Black and Primary Yellow).
* **Body Text:** Regular weight.

## 2. Layout Structure (Desktop)

### A. Navigation Bar (Header)

* **Logo:** The "Ngopi Bareng Teman" line-art logo featuring two faces, a yellow circle accent, and hand-drawn text (top-left).
* **Links:** Centered inline navigation (`Home` \[active with thick yellow underline\], `Flavour`, `Menu`, `About`, `Contact`).
* **Actions:** Right-aligned icons.
  * Search Icon (Black).
  * Shopping Cart Icon with a notification badge (Yellow badge, black text).
  * Primary CTA Button: "Order Now" (Solid Yellow background, Black text, pill shape).

### B. Hero Section

* **Left Column (Content):**
  * Eyebrow text: "NGOPI BARENG TEMAN" (all caps, spaced).
  * Main Headline: "Coffee for a Better Day" (Large typography, "Better Day" highlighted with a yellow background stroke or text color).
  * Description: Short paragraph describing the coffee experience.
  * Action Buttons:
    1. "Order Now ->" (Solid primary yellow, black text, with right arrow).
    2. "Explore Menu" (Outline black button with black text).
  * Feature Highlights (horizontal list with black outline icons):
    * Premium Beans (Leaf icon)
    * Unique Flavours (Cup icon)
    * Freshly Brewed (Heart icon)

* **Right Column (Imagery):**
  * 3D composition featuring three distinct beverage cups on cylindrical pedestals.
  * Floating coffee beans.
  * A circular graphic stamp: "PREMIUM COFFEE BEANS" (Black text on a subtle yellow or white circle).
  * Stylized script/marker text in the background: "Rich Flavour" (in subtle gray or yellow).

### C. Category Menu (Floating Bar)

* A rounded, white floating container overlapping the bottom of the hero section.
* Contains a horizontal list of categories with minimalist black icons:
  * `All Menu` (Active state: light yellow background, black text/icon)
  * `Coffee`
  * `Non Coffee`
  * `Signature`
  * `Cold Drinks`
  * `Pastry`
* "View All ->" link on the far right.

### D. "Our Favourite" Section (Product Carousel)

* **Section Header:**
  * Title: "Our Favourite" (Bold, black).
  * Subtitle: "Handpicked drinks for your daily dose of happiness."
  * Navigation: Left and Right circular arrow buttons (Right button is solid yellow with black arrow, left is outlined black).

* **Product Cards:**
  * White background, rounded corners, soft shadow.
  * Image: High-quality product image at the top with a subtle warm background inside the card image area.
  * Badges: Overlapping the top-left of the image (e.g., "Best Seller" in yellow, "New" in black).
  * Content:
    * Product Name (Bold, black).
    * Short description (Secondary gray text).
    * Price (e.g., "Rp 28.000").
  * Action: Floating "+" button (Solid yellow, black plus icon, bottom right corner).

## 3. Mobile View Variations (Responsive Design)

* **Header:** Links are hidden. Replaced by a Hamburger Menu icon (Black) on the top right.
* **Hero Section:**
  * Content stacks vertically.
  * Typography sizes are scaled down.
  * Buttons span wider or stack if necessary.
  * Imagery is shifted to the bottom of the hero text or scaled down to fit the viewport.
* **Category Menu:** Becomes a horizontally scrollable container. Labels are kept under icons.
* **Our Favourite Section:**
  * Product cards are vertically stacked or horizontally swipable (carousel format).
  * Card width takes up ~80% of the screen width to indicate horizontal scrolling.

## 4. Required Assets

* **Images:** Logo asset "Ngopi Bareng Teman", 3D rendered cups, floating coffee beans, individual product shots.
* **Icons (SVG preferred):** Search, Cart, Leaf, Cup, Heart, Hamburger Menu, Arrow Right, Arrow Left, Plus (+). Ensure all icons follow a consistent line-art style similar to the logo.