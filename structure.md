Structure

assets
app: if using redux
	store.js
	hooks.js ( if ts )
features: if using redux. A reducer equal to a feature
	posts
		posts.slice.ts
		posts.test.ts
		components
			postsList
				PostsList.tsx
				PostsList.styles.scss
			PostItem
				PostItem.tsx
				PostItem.styles.scss
context:
	test.context.ts
components: shared components
	Header
		Header.tsx
		Header.styles.scss
		Header.test.ts
views:
	LoginPage
		LoginPage.tsx
utils:
	constants:
		countries.constants.ts
	helpers:
		validations.helpers.ts
