# Adding Search and Pagination in Next.js

## Introduction

Implementing search and pagination enhances the user experience by allowing users to find and navigate content easily.

## Search Implementation

### Example

```javascript
import { useState } from 'react';

export default function SearchPage({ data }) {
  const [searchTerm, setSearchTerm] = useState('');

  const filteredData = data.filter(item =>
    item.name.toLowerCase().includes(searchTerm.toLowerCase())
  );

  return (
    <div>
      <input
        type="text"
        placeholder="Search..."
        value={searchTerm}
        onChange={e => setSearchTerm(e.target.value)}
      />
      <ul>
        {filteredData.map(item => (
          <li key={item.id}>{item.name}</li>
        ))}
      </ul>
    </div>
  );
}

export async function getStaticProps() {
  const res = await fetch('https://api.example.com/data');
  const data = await res.json();

  return {
    props: { data },
  };
}
```

### Benefits

- **User Experience**: Allows users to quickly find relevant content.

## Pagination Implementation

### Example

```javascript
import { useState } from 'react';

export default function PaginatedPage({ data }) {
  const [currentPage, setCurrentPage] = useState(1);
  const itemsPerPage = 10;

  const indexOfLastItem = currentPage * itemsPerPage;
  const indexOfFirstItem = indexOfLastItem - itemsPerPage;
  const currentItems = data.slice(indexOfFirstItem, indexOfLastItem);

  const paginate = pageNumber => setCurrentPage(pageNumber);

  return (
    <div>
      <ul>
        {currentItems.map(item => (
          <li key={item.id}>{item.name}</li>
        ))}
      </ul>
      <Pagination
        itemsPerPage={itemsPerPage}
        totalItems={data.length}
        paginate={paginate}
      />
    </div>
  );
}

function Pagination({ itemsPerPage, totalItems, paginate }) {
  const pageNumbers = [];

  for (let i = 1; i <= Math.ceil(totalItems / itemsPerPage); i++) {
    pageNumbers.push(i);
  }

  return (
    <nav>
      <ul className="pagination">
        {pageNumbers.map(number => (
          <li key={number} className="page-item">
            <a onClick={() => paginate(number)} className="page-link">
              {number}
            </a>
          </li>
        ))}
      </ul>
    </nav>
  );
}

export async function getStaticProps() {
  const res = await fetch('https://api.example.com/data');
  const data = await res.json();

  return {
    props: { data },
  };
}
```

### Benefits

- **User Experience**: Improves navigation through large sets of data.
- **Performance**: Reduces the amount of data rendered on each page, improving load times.

## Conclusion

Adding search and pagination in Next.js applications significantly enhances user experience by making content more accessible and manageable.

For detailed information, visit [Next.js Adding Search and Pagination](https://nextjs.org/learn/dashboard-app/adding-search-and-pagination).