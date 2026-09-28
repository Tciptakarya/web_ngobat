---
name: Warm Social Cafe
colors:
  surface: '#fbf9f8'
  surface-dim: '#dbdad9'
  surface-bright: '#fbf9f8'
  surface-container-lowest: '#ffffff'
  surface-container-low: '#f5f3f3'
  surface-container: '#efeded'
  surface-container-high: '#e9e8e7'
  surface-container-highest: '#e4e2e2'
  on-surface: '#1b1c1c'
  on-surface-variant: '#4b4731'
  inverse-surface: '#303031'
  inverse-on-surface: '#f2f0f0'
  outline: '#7c775f'
  outline-variant: '#cdc7aa'
  surface-tint: '#6a5f00'
  primary: '#6a5f00'
  on-primary: '#ffffff'
  primary-container: '#ffe600'
  on-primary-container: '#726600'
  inverse-primary: '#dec800'
  secondary: '#5f5e5e'
  on-secondary: '#ffffff'
  secondary-container: '#e5e2e1'
  on-secondary-container: '#656464'
  tertiary: '#006a6a'
  on-tertiary: '#ffffff'
  tertiary-container: '#00feff'
  on-tertiary-container: '#007272'
  error: '#ba1a1a'
  on-error: '#ffffff'
  error-container: '#ffdad6'
  on-error-container: '#93000a'
  primary-fixed: '#fde400'
  primary-fixed-dim: '#dec800'
  on-primary-fixed: '#201c00'
  on-primary-fixed-variant: '#504700'
  secondary-fixed: '#e5e2e1'
  secondary-fixed-dim: '#c8c6c5'
  on-secondary-fixed: '#1c1b1b'
  on-secondary-fixed-variant: '#474646'
  tertiary-fixed: '#00fbfc'
  tertiary-fixed-dim: '#00dcdd'
  on-tertiary-fixed: '#002020'
  on-tertiary-fixed-variant: '#004f50'
  background: '#fbf9f8'
  on-background: '#1b1c1c'
  surface-variant: '#e4e2e2'
  background-base: '#FFFDF5'
  surface-card: '#FFFFFF'
  text-primary: '#111111'
  text-secondary: '#666666'
  tag-bestseller: '#FFE600'
  tag-new: '#111111'
typography:
  display-hero:
    fontFamily: Quicksand
    fontSize: 56px
    fontWeight: '700'
    lineHeight: 64px
  display-hero-mobile:
    fontFamily: Quicksand
    fontSize: 36px
    fontWeight: '700'
    lineHeight: 44px
  headline-lg:
    fontFamily: Quicksand
    fontSize: 32px
    fontWeight: '700'
    lineHeight: 40px
  headline-lg-mobile:
    fontFamily: Quicksand
    fontSize: 26px
    fontWeight: '700'
    lineHeight: 32px
  headline-md:
    fontFamily: Quicksand
    fontSize: 22px
    fontWeight: '700'
    lineHeight: 28px
  headline-sm:
    fontFamily: Quicksand
    fontSize: 18px
    fontWeight: '600'
    lineHeight: 24px
  body-lg:
    fontFamily: Plus Jakarta Sans
    fontSize: 18px
    fontWeight: '400'
    lineHeight: 28px
  body-md:
    fontFamily: Plus Jakarta Sans
    fontSize: 15px
    fontWeight: '400'
    lineHeight: 22px
  body-sm:
    fontFamily: Plus Jakarta Sans
    fontSize: 13px
    fontWeight: '400'
    lineHeight: 18px
  label-lg:
    fontFamily: Plus Jakarta Sans
    fontSize: 14px
    fontWeight: '700'
    lineHeight: 20px
  label-md:
    fontFamily: Plus Jakarta Sans
    fontSize: 12px
    fontWeight: '600'
    lineHeight: 16px
  label-sm:
    fontFamily: Plus Jakarta Sans
    fontSize: 10px
    fontWeight: '700'
    lineHeight: 14px
rounded:
  sm: 0.5rem
  DEFAULT: 1rem
  md: 1.5rem
  lg: 2rem
  xl: 3rem
  full: 9999px
spacing:
  gutter: 1.5rem
  gutter-mobile: 1rem
  margin: 2rem
  margin-mobile: 1rem
  space-xs: 0.25rem
  space-sm: 0.5rem
  space-md: 1rem
  space-lg: 1.5rem
  space-xl: 2.5rem
---

# UI/UX Design Specification: Ngopi Bareng Teman

This document outlines the design specifications for the "Ngopi Bareng Teman" web and mobile application, adapted to match the brand's primary yellow and black identity.

## 1. Global Styles

### Colors
* **Primary Brand/Accent Color:** Bright Yellow (`#FFE600`) - Used for main CTA buttons, highlighted text, active states, and decorative graphic accents.
* **Background Color:** Clean Off-White or Very Pale Cream (`#FAFAFA` or `#FFFDF5`) - Used for the main body background to keep the interface looking clean and let the yellow pop.
* **Card Background:** Pure White (`#FFFFFF`) - Used for product cards and floating category bars to maintain contrast.
* **Text Primary:** Solid Black or Dark Charcoal (`#111111`) - Matches the line art of the logo. Used for headings, primary body text, and text inside primary yellow buttons.
* **Text Secondary:** Medium Gray (`#666666`) - Used for subtitles and descriptions.
* **Tag Backgrounds:**
  * Best Seller: Primary Bright Yellow (`#FFE600`) with Black text.
  * New: Black (`#111111`) with White text.

### Typography
* **Font Family:** Quicksand, Nunito, Inter, Poppins
* **Headings:** Bold and heavy weight. Mixed colors (Black and Primary Yellow).
* **Body Text:** Regular weight.

## 2. Layout Structure (Desktop)
* Navigation Bar with line-art logo, centered links (Home active with thick yellow underline, Flavour, Menu, About, Contact), Search icon, Cart badge, and "Order Now" pill button.
* Hero Section with headline "Coffee for a Better Day", badges (Premium Beans, Unique Flavours, Freshly Brewed), 3D cup imagery and floating coffee beans.
* Category Menu floating bar (All Menu, Coffee, Non Coffee, Signature, Cold Drinks, Pastry).
* "Our Favourite" product carousel cards with tags, descriptions, prices in Rp, and yellow plus buttons.
