# `react-signal-store`
Fast, cross-component observable mutable store library designed with minimalism and zero boilerplate in mind.

## Installation

```sh
# npm
npm install react-signal-store

# Yarn
yarn add react-signal-store
```

## Usage

First, start creating a hook for your store:

```jsx
import { createStoreHook } from "react-signal-store";

export const useBookStore = createStoreHook({
  initialState: {
    books: [
      { id: 1, title: "To Kill a Mockingbird", author: "Harper Lee", genre: "Fiction" },
      { id: 2, title: "1984", author: "George Orwell", genre: "Science Fiction" },
      { id: 3, title: "The Great Gatsby", author: "F. Scott Fitzgerald", genre: "Fiction" },
      { id: 4, title: "Pride and Prejudice", author: "Jane Austen", genre: "Romance" },
    ],
  },
  mutations: ({ state, merge, reset, set }) => ({
    addBook: (newBook) => {
      merge({ books: [...state.books, newBook] });
    },
    removeBook: (bookId) => {
      const updatedBooks = state.books.filter(book => book.id !== bookId);
      set('books', updatedBooks);
    },
    updateBook: (bookId, updatedFields) => {
      const updatedBooks = state.books.map(book => {
        if (book.id === bookId) {
          return { ...book, ...updatedFields };
        }
        return book;
      });
      set('books', updatedBooks);
    },
  }),
});
```
Then you can assert:

```jsx
import React from 'react';
import { useBookStore } from './useBookStore';

function Component() {
  const [bookStore, { addBook }] = useBookStore();

  return (
    <div>
      <h2>Books</h2>
      <ul>
        {bookStore.books.map(book => (
          <li key={book.id}>{book.title} by {book.author}</li>
        ))}
      </ul>
      <button onClick={() => addBook({ id: 5, title: "New Book", author: "New Author", genre: "New Genre" })}>
        Add Book
      </button>
    </div>
  );
}
```