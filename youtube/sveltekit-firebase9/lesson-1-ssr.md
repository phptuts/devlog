# SSR with SvelteKit using Firebase 9 & Firestore

## Code

https://github.com/phptuts/sveltekit-firebase9/tree/1-ssr

## How it works

![Firebase 9 SSR (1)](https://user-images.githubusercontent.com/9620015/157374390-e8cfd7c0-929d-4ffd-a46b-911a64f5abab.png)

## Files


### src -> routes -> index.svelte

This is the file that controls our home page.  We try to load the books here.

```svelte
<script context="module">
	import { getBooks } from '../db';

	export async function load() {
		try {
      // box 2
			const books = await getBooks();

			return {
				props: {
					books
				}
			};
		} catch (e) {
			return {
				status: 500,
				error: new Error('Error Fetching Data From Firebase')
			};
		}
	}
</script>

<script>
	export let books = [];
</script>

<h1>Books Are Awesome</h1>

<ul>
	{#each books as book}
		<li>{book.title} / {book.author}</li>
	{/each}
</ul>

```

### src -> env.ts

This contains our firebase config.

```ts
export default {
	firebase: {
    // ...
	}
};
```

### src -> env.ts

This contains our firebase config.

```ts
export default {
	firebase: {
    // ...
	}
};
```

### src -> firebase.ts

This will initialize firebase and give us access any firebase library we are using in the app.

```ts
import { initializeApp, getApps } from 'firebase/app';
import config from './env';
import { getFirestore } from 'firebase/firestore';

export function getFirebase() {
	if (getApps().length == 0) {
		initializeApp(config.firebase);
	}

	return { db: getFirestore() };
}
```

### src -> db.ts

This will contain all our firestore database calls.

```ts
import { collection, getDocs } from '@firebase/firestore';
import { getFirebase } from './firebase';
import type { Book } from './model';

export async function getBooks(): Promise<Book[]> {
	const { db } = getFirebase();
	const colRef = collection(db, 'books');
	const snapshot = await getDocs(colRef);

	return snapshot.docs.map((b) => {
		const data = b.data();

		return {
			id: b.id,
			title: data['title'],
			author: data['author']
		};
	});
}
```

### src -> model.ts

This contain all our interfaces for interacting with firestore.

```ts
export interface Book {
	title: string;
	id: string;
	author: string;
}
```

