---
name: restaurant-dish-recommender
description: Recommend personalized dishes and cocktails at a restaurant based on the diner's tastes, the official menu, Google Maps reviews, and editorial reviews. Use whenever a user asks what to order at a restaurant, asks for dish, drink, or menu recommendations, or wants personalized food and cocktail picks for a dining spot.
allowed-tools: google google_maps_tool image_gen gemini_personalization_agent context_service_agent
---
# Restaurant Dish Recommender

Recommend personalized dishes and cocktails from a restaurant's menu by combining the user's flavor and beverage preferences with menu verification, Google Maps diner reviews, and professional editorial reviews, presented with a headline infographic and structured explanations.

## When to Use

- The user asks for dish or drink recommendations at a specific restaurant
- The user asks "what should I get at [restaurant]?" or "what is good to order at [restaurant]?"
- Someone in a foodie chat or dining group wants food or cocktail picks tailored to a specific person's palate rather than generic crowd favorites
- The user shares a restaurant name and their taste preferences (or asks you to infer their tastes) to decide what to eat and drink

## Steps

### 1. Resolve the Restaurant Entity

Identify the exact restaurant and location before researching dishes:

- If the restaurant name is ambiguous and one of the plausible matches is a kosher restaurant in New Jersey or New York, default to that kosher NJ/NY location (for example, resolve "Middle Feast" to Middle Feast in Teaneck, NJ).
- Use `google_maps_tool` (`find_places`) and `google` (`search`) to verify the restaurant's exact location, operating status, and cuisine style.
- If multiple kosher NJ/NY locations exist with distinctly different menus, or if no NJ/NY kosher match exists and the location remains ambiguous, ask a brief clarifying question.

### 2. Gather User Taste and Drink Preferences

Tailor recommendations to the diner's specific palate and beverage tastes:

- Check the user's prompt for stated food and drink preferences (preferred spirits, flavor profiles, spice tolerance, sweetness levels, favorite proteins, or dietary restrictions).
- Check personal context and past preferences using `gemini_personalization_agent` or `context_service_agent` if preferences are not explicitly stated in the prompt.
- If you have no information about the diner's tastes from the prompt or personal context, ask 1 to 2 quick, concrete questions about what flavors, foods, and drinks they enjoy before generating the final recommendations (or if they ask for immediate picks, state the taste profile you are targeting).

### 3. Research the Menu, Cocktails, and Reviews

Verify current dishes and drinks and cross-reference public consensus with the diner's tastes:

- **Menu Verification:** Search and browse the restaurant's official website or current online menu via `google` (`search` and `browse`) to confirm exact dish names, ingredients, and availability. Verify whether the restaurant maintains a dedicated cocktail or bar menu. Never recommend a dish or drink that is not on the restaurant's menu.
- **Google Maps Reviews:** Check `google_maps_tool` and search snippets for frequently praised dishes, cocktails, preparation notes, and diner feedback.
- **Editorial and Community Reviews:** Search the web for professional food critic reviews, bar write-ups, local dining guides, and kosher foodie community discussions to identify signature cocktails and standout items.
- **Match to Palate:** Filter the top-reviewed dishes and cocktails through the user's taste and drink profile so every pick aligns with what they actually enjoy.

### 4. Select Items by Category

Choose the strongest matches from the verified menu across these categories:

- **Appetizer:** 1 to 2 recommended dishes
- **Entree:** 1 to 2 recommended dishes
- **Dessert:** 1 to 2 recommended dishes
- **Cocktails (Conditional):** 1 to 2 recommended signature cocktails if the restaurant has a cocktail menu or bar program, matched to the diner's preferred spirits and flavor profiles. Omit this category entirely if the restaurant does not offer cocktails.
- **Bonus (Optional):** 1 to 2 notable dishes that do not fit the standard categories (such as a signature sushi roll, standout side dish, specialty bread, or unique beverage), included only when truly notable.
- **Omit Non-Applicable Categories:** If a restaurant does not serve a category (for example, no desserts or no cocktails), omit that category completely rather than forcing a weak match.

### 5. Generate the Headline Infographic and Formatted Output

Present the recommendations with a visual overview followed by scannable explanations:

- **Headline Infographic:** Call `image_gen` (`generate_image`) to create a clean, appetizing visual infographic titled with the restaurant name that showcases the recommended dishes and cocktails organized by their categories (Appetizer, Entree, Dessert, Cocktails if applicable, and Bonus if applicable) with exact menu names spelled out clearly. Embed the generated image at the very top of your response using `![Restaurant Dish Recommendations](filename)`.
- **Formatted Category Breakdown:** Below the infographic, list each applicable category (`### Cocktails`, `### Appetizers`, `### Entrees`, `### Desserts`, and optional `### Bonus`).
- **Brief Explanations:** For each recommendation, provide the exact menu name, why it fits the user's specific taste and drink preferences, and supporting evidence from the menu, Google Maps reviews, or editorial reviews with inline source links.

## Gotchas

- **Recommending cocktails without a cocktail menu:** Verify whether the restaurant actually serves cocktails before including drink picks. If it's a BYOB, beer/wine only, or casual counter spot with no mixed drinks, cleanly omit the Cocktails section.
- **Recommending generic favorites over personal fit:** Do not just dump the top most-reviewed items if they conflict with the user's palate. Always explain the connection between the dish or drink and the user's tastes.
- **Missing the NJ/NY kosher default:** Remember that the primary audience is kosher diners in New Jersey and New York. Always check for a NJ/NY kosher restaurant match first before assuming a national chain or out-of-state venue with a similar name.
- **Hallucinating menu items:** Verify that every recommended dish and cocktail actually exists on the restaurant's menu.
- **Skipping the infographic:** Always generate and embed the headline infographic image at the top of the response before the text breakdown.
