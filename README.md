# Myexp# Myexp
import React, { useState } from 'react';

const App = () => {
  // Sample data to filter
  const sampleData = [
    { id: 1, name: 'John Doe', age: 30, email: 'john@example.com' },
    { id: 2, name: 'Jane Smith', age: 25, email: 'jane@example.com' },
    { id: 3, name: 'Alice Johnson', age: 35, email: 'alice@example.com' },
    { id: 4, name: 'Bob Brown', age: 40, email: 'bob@example.com' },
  ];

  return (
    <div style={{ padding: '20px', fontFamily: 'Arial, sans-serif' }}>
      <h1>Dynamic Filter App</h1>
      <FilterComponent data={sampleData} />
    </div>
  );
};

const FilterComponent = ({ data }) => {
  const [filters, setFilters] = useState([{ id: Date.now(), field: '', operator: '', value: '' }]);

  // Available filter options
  const availableFields = [
    { label: 'Name', value: 'name' },
    { label: 'Age', value: 'age' },
    { label: 'Email', value: 'email' },
  ];

  const availableOperators = [
    { label: 'Equals', value: 'eq' },
    { label: 'Contains', value: 'contains' },
    { label: 'Greater than', value: 'gt' },
    { label: 'Less than', value: 'lt' },
  ];

  // Add new filter
  const addFilter = () => {
    setFilters([...filters, { id: Date.now(), field: '', operator: '', value: '' }]);
  };

  // Remove filter
  const removeFilter = (id) => {
    if (filters.length > 1) {
      setFilters(filters.filter(filter => filter.id !== id));
    }
  };

  // Update filter values
  const updateFilter = (id, field, value) => {
    setFilters(filters.map(filter =>
      filter.id === id ? { ...filter, [field]: value } : filter
    ));
  };

  // Filter the data based on active filters
  const getFilteredData = () => {
    return data.filter(item => {
      return filters.every(filter => {
        if (!filter.field || !filter.operator || !filter.value) return true;

        const itemValue = item[filter.field];
        const filterValue = filter.value;

        switch (filter.operator) {
          case 'eq':
            return itemValue == filterValue;
          case 'contains':
            return String(itemValue).toLowerCase().includes(filterValue.toLowerCase());
          case 'gt':
            return Number(itemValue) > Number(filterValue);
          case 'lt':
            return Number(itemValue) < Number(filterValue);
          default:
            return true;
        }
      });
    });
  };

  const filteredData = getFilteredData();

  return (
    <div>
      <div className="filters" style={{ marginBottom: '20px' }}>
        {filters.map((filter) => (
          <div key={filter.id} style={{ marginBottom: '10px', display: 'flex', gap: '10px', alignItems: 'center' }}>
            <select
              value={filter.field}
              onChange={(e) => updateFilter(filter.id, 'field', e.target.value)}
              style={{ padding: '5px' }}
            >
              <option value="">Select Field</option>
              {availableFields.map(field => (
                <option key={field.value} value={field.value}>{field.label}</option>
              ))}
            </select>

            <select
              value={filter.operator}
              onChange={(e) => updateFilter(filter.id, 'operator', e.target.value)}
              style={{ padding: '5px' }}
            >
              <option value="">Select Operator</option>
              {availableOperators.map(op => (
                <option key={op.value} value={op.value}>{op.label}</option>
              ))}
            </select>

            <input
              type="text"
              value={filter.value}
              onChange={(e) => updateFilter(filter.id, 'value', e.target.value)}
              placeholder="Value"
              style={{ padding: '5px' }}
            />

            <button
              onClick={() => removeFilter(filter.id)}
              style={{ padding: '5px 10px', backgroundColor: '#ff4444', color: 'white', border: 'none', borderRadius: '4px', cursor: 'pointer' }}
            >
              Remove
            </button>
          </div>
        ))}
      </div>

      <button
        onClick={addFilter}
        style={{ padding: '10px 20px', backgroundColor: '#007bff', color: 'white', border: 'none', borderRadius: '4px', cursor: 'pointer', marginBottom: '20px' }}
      >
        Add Filter
      </button>

      <div className="results">
        <h3>Results ({filteredData.length})</h3>
        <ul style={{ listStyle: 'none', padding: '0' }}>
          {filteredData.map(item => (
            <li key={item.id} style={{ padding: '10px', border: '1px solid #ddd', marginBottom: '5px', borderRadius: '4px' }}>
              <strong>{item.name}</strong> - {item.age} years - {item.email}
            </li>
          ))}
        </ul>
      </div>
    </div>
  );
};

export default App;
