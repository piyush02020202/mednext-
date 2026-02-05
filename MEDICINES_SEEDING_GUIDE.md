# Seeding Medicines to MedNext+ Database

This guide explains how to populate your MedNext+ database with a comprehensive list of Indian medicines in Indian Rupees (₹).

## What's Included

The seed file includes **21 medicines** across 7 categories:
- **Antibiotics** (3 medicines)
- **Vitamins** (3 medicines)
- **Pain Relief** (3 medicines)
- **Cold & Flu** (3 medicines)
- **Digestive** (3 medicines)
- **Skin Care** (3 medicines)
- **Supplements** (3 medicines)

All medicines are priced in **Indian Rupees (₹)** with realistic pricing for the Indian market.

## How to Seed the Database

### Prerequisites
- Ensure MongoDB is running
- Ensure your `.env` file is configured with a valid `MONGODB_URI`
- Node.js and npm are installed

### Steps to Run the Seed

1. Navigate to the backend directory:
   ```bash
   cd backend
   ```

2. Install dependencies (if not already done):
   ```bash
   npm install
   ```

3. Run the seed script:
   ```bash
   npm run seed
   ```

The script will:
- Connect to your MongoDB database
- Clear any existing products
- Insert all 21 medicines with complete details
- Confirm successful completion

## Seed Data Details

Each medicine includes:
- **Name**: Medicine name with strength (e.g., "Amoxicillin 500mg")
- **Description**: What the medicine is used for
- **Price**: In Indian Rupees (₹)
- **Discount**: Percentage discount available (5-15%)
- **Category**: One of 7 predefined categories
- **Manufacturer**: Indian pharmaceutical company
- **Stock**: Quantity available
- **Requires Prescription**: Boolean flag for prescription requirement
- **Image**: Placeholder image URL
- **Rating**: 4.3 to 4.6 out of 5.0

## Sample Medicines

### Antibiotics
- Amoxicillin 500mg - ₹45
- Azithromycin 250mg - ₹65
- Cephalexin 500mg - ₹55

### Vitamins
- Vitamin D3 1000IU - ₹120
- Vitamin B Complex - ₹95
- Vitamin C 500mg - ₹80

### Pain Relief
- Ibuprofen 400mg - ₹35
- Paracetamol 500mg - ₹30
- Aspirin 75mg - ₹40

... and more medicines in other categories.

## Verification

After running the seed, you can verify the medicines were added by:

1. **Check Backend Logs**: You should see a message like:
   ```
   Successfully added 21 medicines to the database
   Medicines added with Indian currency (₹)
   ```

2. **Check via API**: Make a GET request to:
   ```
   http://localhost:5000/api/products
   ```

3. **Check MongoDB**: Connect to your MongoDB instance and query the products collection

## Currency Format

The Indian Rupee symbol (₹) is already integrated throughout the frontend:
- ProductCard displays: `₹{product.price}`
- Cart displays: `₹{item.price * item.quantity}`
- Checkout displays: `₹{getTotalPrice()}`

## Adding More Medicines

To add more medicines, edit `backend/src/utils/seedMedicines.js` and add entries to the `medicines` array following the same structure.

## Resetting the Database

Running `npm run seed` will clear all existing products and add the 21 medicines. If you want to preserve existing data, modify the seed file to comment out the `await Product.deleteMany({});` line.

## Troubleshooting

- **MongoDB Connection Error**: Verify `MONGODB_URI` in your `.env` file
- **Module Not Found**: Ensure all dependencies are installed with `npm install`
- **Permission Errors**: Check that MongoDB is running and you have proper connection credentials
