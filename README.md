# SuperJSONTree

A specialized component for visualizing and exploring JSON data structures in an interactive tree format, providing clear hierarchical representation of complex object data for Budibase applications.

## 🚀 Features

### JSON Visualization

- **Interactive Tree Display**: Expandable/collapsible JSON structure representation
- **Value Type Highlighting**: Color-coded display of different data types (string, number, boolean, object, array)
- **Nested Object Support**: Deep nesting visualization with proper indentation
- **Array Indexing**: Clear array element indexing and counting

### Display Options

- **Key Count Display**: Show/hide count of keys in objects and arrays
- **Quiet Mode**: Subtle styling for secondary data display
- **Monospace Font**: Use fixed-width font for better alignment
- **Visual Hierarchy**: Clear parent-child relationship representation

## 🎯 Usage Instructions

### Basic Setup

1. Add the SuperJSONTree component to your screen
2. Bind the JSON data source to the Value field
3. Configure display preferences (key count, styling)
4. Test the tree expansion to ensure proper data visualization

### Data Source Configuration

- **Value Field**: Bind to any JSON object or array data source
- **Dynamic Content**: Supports binding to form data, API responses, or component context
- **Type Validation**: Automatically handles different JSON data types
- **Null Handling**: Graceful display of null values and empty objects

### Display Customization

#### Visual Settings

- **Key Count**: Display number of properties for objects/arrays
- **Quiet Documents**: Enable subtle background and typography
- **Monospace**: Consistent character spacing for data alignment
- **Hierarchical Depth**: Visual tree structure representation

## 🔧 Configuration Properties

### Data Configuration

- **Value**: JSON object or array to display (template/binding support)

### Display Options

- **Show Key Count**: Display property counts for objects and arrays
- **Quiet**: Enable subtle, secondary visual styling
- **Monospace**: Use fixed-width font for better data alignment

## 📋 Usage Examples

### API Response Viewer

Configure to display REST API responses:

- Value: `{{ apiResponse.data }}`
- Show Key Count: Enabled
- Monospace: Enabled
- Quiet: Disabled

### Form Data Inspector

Display form data structure during development:

- Value: `{{ form.values }}`
- Show Key Count: Enabled
- Monospace: Enabled
- Quiet: Enabled

### Configuration Object Viewer

Display application settings or user preferences:

- Value: `{{ user.config }}`
- Show Key Count: Enabled
- Monospace: Disabled
- Quiet: Disabled

## 🎨 Styling & Theming

### Typography

- **Monospace Mode**: Fixed-width font for data alignment
- **Variable Font**: Default: Application standard typography
- **Size Scaling**: Automatic sizing based on font settings
- **Readability**: Optimized contrast and spacing

### Visual Elements

- **Node Icons**: Different icons for objects, arrays, primitives
- **Expandable Indicators**: Plus/minus icons for expandable nodes
- **Color Coding**: Type-based color highlighting
- **Hover Effects**: Interactive feedback on expandable nodes

## 🔗 Integration

### Data Sources

- **API Responses**: Display REST API response structures
- **Form Data**: Visualize form submission data
- **Configuration Objects**: Show application settings
- **Database Records**: Display complex document structures

### Context Integration

- **Current Record**: Bind to `{{ row }}` for document inspection
- **Form Values**: Connect to `{{ form.values }}` for data preview
- **API Results**: Link to `{{ apiResponse }}` for response inspection
- **Component Context**: Access to parent component data structures

## 📱 Responsiveness & Accessibility

### Responsive Design

- **Container Adaptation**: Fits within parent container dimensions
- **Content Scrolling**: Horizontal/vertical scrolling for large structures
- **Font Scaling**: Adequate font sizes across screen sizes
- **Touch Interaction**: Touch-friendly interaction for mobile devices

### Accessibility Features

- **Screen Reader Support**: Proper semantic markup for assistive technologies
- **Keyboard Navigation**: Arrow key navigation through tree structure
- **Focus Indicators**: Clear focus highlights on interactive elements
- **Label Associations**: Proper field labeling and descriptions

## 🐛 Troubleshooting

### Data Display Issues

- **Empty Display**: Verify JSON value binding and data structure
- **Malformed JSON**: Ensure valid JSON structure
- **Large Objects**: Performance issues with deeply nested/large objects
- **Circular References**: Avoid objects with circular references

### Visual Issues

- **Typography**: Check monospace/regular font settings
- **Styling**: Verify quiet mode and theme compatibility
- **Spacing**: Adjust padding and container dimensions
- **Visibility**: Ensure component is not hidden or collapsed

### Performance Considerations

- **Large Structures**: Limit display size for better performance
- **Deep Nesting**: Consider pagination for deeply nested objects
- **Update Frequency**: Minimize updates for smooth interaction
- **Memory Usage**: Monitor memory usage for large JSON objects

Find out more about developing [Custom Plugins](https://docs.budibase.com/docs/custom-plugin) and [Budibase](https://github.com/Budibase/budibase).
