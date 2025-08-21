# Component Library & Design System

## Version 1.0.0 | January 2025

## 🎨 Design Tokens

### Color Palette

```typescript
export const colors = {
  // Primary - Professional Blue
  primary: {
    50: '#EFF6FF',
    100: '#DBEAFE',
    200: '#BFDBFE',
    300: '#93C5FD',
    400: '#60A5FA',
    500: '#3B82F6', // Main
    600: '#2563EB',
    700: '#1D4ED8',
    800: '#1E40AF',
    900: '#1E3A8A',
  },
  
  // Secondary - Success Green
  secondary: {
    50: '#F0FDF4',
    100: '#DCFCE7',
    200: '#BBF7D0',
    300: '#86EFAC',
    400: '#4ADE80',
    500: '#22C55E', // Main
    600: '#16A34A',
    700: '#15803D',
    800: '#166534',
    900: '#14532D',
  },
  
  // Neutral - Gray Scale
  neutral: {
    50: '#F9FAFB',
    100: '#F3F4F6',
    200: '#E5E7EB',
    300: '#D1D5DB',
    400: '#9CA3AF',
    500: '#6B7280',
    600: '#4B5563',
    700: '#374151',
    800: '#1F2937',
    900: '#111827',
  },
  
  // Semantic Colors
  semantic: {
    error: '#EF4444',
    warning: '#F59E0B',
    info: '#3B82F6',
    success: '#22C55E',
  },
  
  // Scoring Colors
  scoring: {
    excellent: '#10B981', // 90-100%
    good: '#22C55E',      // 70-89%
    fair: '#F59E0B',      // 50-69%
    poor: '#EF4444',      // Below 50%
  },
};
```

### Typography

```typescript
export const typography = {
  fonts: {
    sans: 'Inter, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif',
    mono: '"JetBrains Mono", "SF Mono", Monaco, monospace',
  },
  
  sizes: {
    xs: '0.75rem',    // 12px
    sm: '0.875rem',   // 14px
    base: '1rem',     // 16px
    lg: '1.125rem',   // 18px
    xl: '1.25rem',    // 20px
    '2xl': '1.5rem',  // 24px
    '3xl': '1.875rem', // 30px
    '4xl': '2.25rem', // 36px
    '5xl': '3rem',    // 48px
  },
  
  weights: {
    normal: 400,
    medium: 500,
    semibold: 600,
    bold: 700,
  },
  
  lineHeights: {
    tight: 1.25,
    normal: 1.5,
    relaxed: 1.75,
  },
};
```

### Spacing

```typescript
export const spacing = {
  0: '0',
  px: '1px',
  0.5: '0.125rem', // 2px
  1: '0.25rem',    // 4px
  2: '0.5rem',     // 8px
  3: '0.75rem',    // 12px
  4: '1rem',       // 16px
  5: '1.25rem',    // 20px
  6: '1.5rem',     // 24px
  8: '2rem',       // 32px
  10: '2.5rem',    // 40px
  12: '3rem',      // 48px
  16: '4rem',      // 64px
  20: '5rem',      // 80px
  24: '6rem',      // 96px
};
```

### Border Radius

```typescript
export const borderRadius = {
  none: '0',
  sm: '0.125rem',  // 2px
  base: '0.25rem', // 4px
  md: '0.375rem',  // 6px
  lg: '0.5rem',    // 8px
  xl: '0.75rem',   // 12px
  '2xl': '1rem',   // 16px
  full: '9999px',
};
```

### Shadows

```typescript
export const shadows = {
  none: 'none',
  sm: '0 1px 2px 0 rgba(0, 0, 0, 0.05)',
  base: '0 1px 3px 0 rgba(0, 0, 0, 0.1), 0 1px 2px 0 rgba(0, 0, 0, 0.06)',
  md: '0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06)',
  lg: '0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05)',
  xl: '0 20px 25px -5px rgba(0, 0, 0, 0.1), 0 10px 10px -5px rgba(0, 0, 0, 0.04)',
  '2xl': '0 25px 50px -12px rgba(0, 0, 0, 0.25)',
  inner: 'inset 0 2px 4px 0 rgba(0, 0, 0, 0.06)',
};
```

### Breakpoints

```typescript
export const breakpoints = {
  xs: '480px',
  sm: '640px',
  md: '768px',
  lg: '1024px',
  xl: '1280px',
  '2xl': '1536px',
};
```

---

## 🧩 Core Components

### 1. Button Component

```typescript
interface ButtonProps {
  variant: 'primary' | 'secondary' | 'outline' | 'ghost' | 'danger';
  size: 'xs' | 'sm' | 'md' | 'lg' | 'xl';
  loading?: boolean;
  disabled?: boolean;
  fullWidth?: boolean;
  leftIcon?: ReactNode;
  rightIcon?: ReactNode;
  onClick?: () => void;
  type?: 'button' | 'submit' | 'reset';
  children: ReactNode;
}

// Usage
<Button 
  variant="primary" 
  size="md"
  leftIcon={<PlusIcon />}
  loading={isLoading}
>
  Create Job
</Button>
```

### 2. Input Component

```typescript
interface InputProps {
  type: 'text' | 'email' | 'password' | 'number' | 'tel' | 'url';
  label?: string;
  placeholder?: string;
  helper?: string;
  error?: string;
  required?: boolean;
  disabled?: boolean;
  prefix?: ReactNode;
  suffix?: ReactNode;
  value: string;
  onChange: (value: string) => void;
}

// Usage
<Input
  type="email"
  label="Email Address"
  placeholder="user@example.com"
  error={errors.email}
  required
  value={email}
  onChange={setEmail}
/>
```

### 3. Select Component

```typescript
interface SelectProps<T> {
  label?: string;
  placeholder?: string;
  options: Array<{
    value: T;
    label: string;
    disabled?: boolean;
  }>;
  value: T | null;
  onChange: (value: T) => void;
  multiple?: boolean;
  searchable?: boolean;
  clearable?: boolean;
  loading?: boolean;
  error?: string;
}

// Usage
<Select
  label="Department"
  placeholder="Select department"
  options={departments}
  value={selectedDept}
  onChange={setSelectedDept}
  searchable
/>
```

### 4. Card Component

```typescript
interface CardProps {
  title?: string;
  subtitle?: string;
  actions?: ReactNode;
  footer?: ReactNode;
  padding?: 'none' | 'sm' | 'md' | 'lg';
  shadow?: 'none' | 'sm' | 'md' | 'lg';
  hoverable?: boolean;
  onClick?: () => void;
  children: ReactNode;
}

// Usage
<Card
  title="Software Engineer"
  subtitle="Engineering Department"
  actions={<Button size="sm">View</Button>}
  hoverable
>
  <p>45 candidates • Active</p>
</Card>
```

### 5. Modal Component

```typescript
interface ModalProps {
  isOpen: boolean;
  onClose: () => void;
  title?: string;
  size?: 'xs' | 'sm' | 'md' | 'lg' | 'xl' | 'full';
  closeOnOverlayClick?: boolean;
  closeOnEsc?: boolean;
  footer?: ReactNode;
  children: ReactNode;
}

// Usage
<Modal
  isOpen={isModalOpen}
  onClose={closeModal}
  title="Upload CVs"
  size="lg"
  footer={
    <>
      <Button variant="ghost" onClick={closeModal}>Cancel</Button>
      <Button variant="primary" onClick={handleUpload}>Upload</Button>
    </>
  }
>
  {/* Modal content */}
</Modal>
```

### 6. Table Component

```typescript
interface TableProps<T> {
  columns: Array<{
    key: keyof T;
    header: string;
    sortable?: boolean;
    width?: string;
    render?: (value: any, row: T) => ReactNode;
  }>;
  data: T[];
  loading?: boolean;
  selectable?: boolean;
  onRowClick?: (row: T) => void;
  onSelectionChange?: (selected: T[]) => void;
  pagination?: {
    page: number;
    pageSize: number;
    total: number;
    onChange: (page: number, pageSize: number) => void;
  };
}

// Usage
<Table
  columns={columns}
  data={candidates}
  loading={isLoading}
  selectable
  onSelectionChange={setSelectedCandidates}
  pagination={{
    page: currentPage,
    pageSize: 20,
    total: totalCandidates,
    onChange: handlePageChange
  }}
/>
```

### 7. Toast/Notification

```typescript
interface ToastProps {
  type: 'success' | 'error' | 'warning' | 'info';
  title: string;
  message?: string;
  duration?: number;
  action?: {
    label: string;
    onClick: () => void;
  };
}

// Usage
toast.success({
  title: 'CV Uploaded',
  message: 'Successfully processed 5 candidates',
  duration: 5000
});
```

### 8. Badge Component

```typescript
interface BadgeProps {
  variant: 'primary' | 'secondary' | 'success' | 'warning' | 'error' | 'neutral';
  size: 'xs' | 'sm' | 'md';
  dot?: boolean;
  rounded?: boolean;
  children: ReactNode;
}

// Usage
<Badge variant="success" size="sm">
  Active
</Badge>
```

### 9. Progress Component

```typescript
interface ProgressProps {
  value: number;
  max?: number;
  size?: 'xs' | 'sm' | 'md' | 'lg';
  color?: 'primary' | 'secondary' | 'success' | 'warning' | 'error';
  showLabel?: boolean;
  label?: string;
  variant?: 'linear' | 'circular';
}

// Usage
<Progress
  value={uploadProgress}
  max={100}
  color="primary"
  showLabel
  label="Uploading..."
/>
```

### 10. Tabs Component

```typescript
interface TabsProps {
  tabs: Array<{
    key: string;
    label: string;
    icon?: ReactNode;
    badge?: number;
    disabled?: boolean;
  }>;
  activeTab: string;
  onChange: (key: string) => void;
  variant?: 'line' | 'enclosed' | 'pills';
}

// Usage
<Tabs
  tabs={[
    { key: 'overview', label: 'Overview' },
    { key: 'candidates', label: 'Candidates', badge: 45 },
    { key: 'analytics', label: 'Analytics' }
  ]}
  activeTab={activeTab}
  onChange={setActiveTab}
/>
```

---

## 📦 Composite Components

### 1. FileUpload Component

```typescript
interface FileUploadProps {
  accept?: string;
  multiple?: boolean;
  maxSize?: number;
  maxFiles?: number;
  onUpload: (files: File[]) => void;
  onError?: (error: string) => void;
  preview?: boolean;
}

// Features:
// - Drag & drop
// - File validation
// - Progress tracking
// - Preview thumbnails
// - Retry on error
```

### 2. DataGrid Component

```typescript
interface DataGridProps<T> {
  // Extends Table with:
  columns: ExtendedColumn<T>[];
  data: T[];
  
  // Advanced features
  filtering?: boolean;
  sorting?: boolean;
  grouping?: boolean;
  exporting?: boolean;
  columnResize?: boolean;
  virtualization?: boolean;
  
  // Callbacks
  onFilterChange?: (filters: Filter[]) => void;
  onSortChange?: (sort: Sort[]) => void;
  onExport?: (format: 'csv' | 'excel' | 'pdf') => void;
}
```

### 3. SearchBar Component

```typescript
interface SearchBarProps {
  placeholder?: string;
  suggestions?: string[];
  filters?: Filter[];
  onSearch: (query: string, filters?: Filter[]) => void;
  onClear?: () => void;
  debounceMs?: number;
  showFilters?: boolean;
}
```

### 4. ScoreCard Component

```typescript
interface ScoreCardProps {
  score: number;
  maxScore?: number;
  criteria: Array<{
    name: string;
    score: number;
    maxScore: number;
    evidence?: string[];
  }>;
  variant?: 'compact' | 'detailed';
  showEvidence?: boolean;
}
```

### 5. StatCard Component

```typescript
interface StatCardProps {
  title: string;
  value: string | number;
  change?: {
    value: number;
    trend: 'up' | 'down';
  };
  icon?: ReactNode;
  chart?: ReactNode;
  loading?: boolean;
}
```

---

## 🎭 Layout Components

### 1. AppShell

```typescript
interface AppShellProps {
  header: ReactNode;
  sidebar?: ReactNode;
  footer?: ReactNode;
  children: ReactNode;
  sidebarCollapsed?: boolean;
  onSidebarToggle?: () => void;
}
```

### 2. PageHeader

```typescript
interface PageHeaderProps {
  title: string;
  subtitle?: string;
  breadcrumbs?: Array<{
    label: string;
    href?: string;
  }>;
  actions?: ReactNode;
  tabs?: ReactNode;
}
```

### 3. EmptyState

```typescript
interface EmptyStateProps {
  icon?: ReactNode;
  title: string;
  description?: string;
  action?: {
    label: string;
    onClick: () => void;
  };
  illustration?: 'no-data' | 'error' | 'search' | 'success';
}
```

### 4. LoadingState

```typescript
interface LoadingStateProps {
  variant: 'spinner' | 'skeleton' | 'progress' | 'dots';
  text?: string;
  fullScreen?: boolean;
}
```

---

## 🔧 Utility Components

### 1. Avatar

```typescript
interface AvatarProps {
  src?: string;
  name?: string;
  size?: 'xs' | 'sm' | 'md' | 'lg' | 'xl';
  shape?: 'circle' | 'square';
  status?: 'online' | 'offline' | 'busy' | 'away';
}
```

### 2. Tooltip

```typescript
interface TooltipProps {
  content: ReactNode;
  placement?: 'top' | 'bottom' | 'left' | 'right';
  trigger?: 'hover' | 'click' | 'focus';
  delay?: number;
  children: ReactNode;
}
```

### 3. Dropdown

```typescript
interface DropdownProps {
  trigger: ReactNode;
  items: Array<{
    key: string;
    label: string;
    icon?: ReactNode;
    danger?: boolean;
    disabled?: boolean;
    onClick?: () => void;
  }>;
  placement?: 'bottom-start' | 'bottom-end' | 'top-start' | 'top-end';
}
```

### 4. Breadcrumb

```typescript
interface BreadcrumbProps {
  items: Array<{
    label: string;
    href?: string;
    icon?: ReactNode;
  }>;
  separator?: ReactNode;
  maxItems?: number;
}
```

---

## 📊 Chart Components

### 1. LineChart

```typescript
interface LineChartProps {
  data: ChartData;
  height?: number;
  showLegend?: boolean;
  showGrid?: boolean;
  interactive?: boolean;
  onPointClick?: (point: DataPoint) => void;
}
```

### 2. BarChart

```typescript
interface BarChartProps {
  data: ChartData;
  orientation?: 'vertical' | 'horizontal';
  stacked?: boolean;
  height?: number;
}
```

### 3. PieChart

```typescript
interface PieChartProps {
  data: Array<{
    label: string;
    value: number;
    color?: string;
  }>;
  donut?: boolean;
  showLabels?: boolean;
  size?: number;
}
```

### 4. FunnelChart

```typescript
interface FunnelChartProps {
  stages: Array<{
    name: string;
    value: number;
    conversion?: number;
  }>;
  height?: number;
  showConversion?: boolean;
}
```

---

## 🎯 Form Components

### 1. FormField

```typescript
interface FormFieldProps {
  name: string;
  label?: string;
  required?: boolean;
  error?: string;
  helper?: string;
  children: ReactNode;
}
```

### 2. RadioGroup

```typescript
interface RadioGroupProps<T> {
  name: string;
  options: Array<{
    value: T;
    label: string;
    description?: string;
    disabled?: boolean;
  }>;
  value: T;
  onChange: (value: T) => void;
  orientation?: 'horizontal' | 'vertical';
}
```

### 3. Checkbox

```typescript
interface CheckboxProps {
  label: string;
  checked: boolean;
  onChange: (checked: boolean) => void;
  indeterminate?: boolean;
  disabled?: boolean;
  description?: string;
}
```

### 4. DatePicker

```typescript
interface DatePickerProps {
  value: Date | null;
  onChange: (date: Date | null) => void;
  placeholder?: string;
  minDate?: Date;
  maxDate?: Date;
  format?: string;
  showTime?: boolean;
}
```

### 5. RangeSlider

```typescript
interface RangeSliderProps {
  min: number;
  max: number;
  value: [number, number];
  onChange: (value: [number, number]) => void;
  step?: number;
  labels?: boolean;
  marks?: Array<{ value: number; label: string }>;
}
```

---

## 🎨 Tailwind Configuration

```javascript
// tailwind.config.js
module.exports = {
  content: ['./src/**/*.{js,ts,jsx,tsx}'],
  theme: {
    extend: {
      colors: {
        primary: colors.primary,
        secondary: colors.secondary,
        neutral: colors.neutral,
        // ... other colors
      },
      fontFamily: {
        sans: ['Inter', 'sans-serif'],
        mono: ['JetBrains Mono', 'monospace'],
      },
      spacing: spacing,
      borderRadius: borderRadius,
      boxShadow: shadows,
    },
  },
  plugins: [
    require('@tailwindcss/forms'),
    require('@tailwindcss/typography'),
    require('@tailwindcss/aspect-ratio'),
  ],
};
```

---

## 🔌 Component Library Setup

### Using Radix UI + Tailwind

```bash
# Install Radix primitives
npm install @radix-ui/react-dialog
npm install @radix-ui/react-dropdown-menu
npm install @radix-ui/react-tabs
npm install @radix-ui/react-tooltip
# ... other primitives

# Install utility libraries
npm install clsx tailwind-merge
npm install @tailwindcss/forms
npm install react-hook-form zod
```

### Component Structure

```
src/
├── components/
│   ├── ui/           # Base UI components
│   │   ├── Button.tsx
│   │   ├── Input.tsx
│   │   ├── Card.tsx
│   │   └── ...
│   ├── composite/    # Complex components
│   │   ├── FileUpload.tsx
│   │   ├── DataGrid.tsx
│   │   └── ...
│   ├── layout/       # Layout components
│   │   ├── AppShell.tsx
│   │   ├── PageHeader.tsx
│   │   └── ...
│   └── charts/       # Chart components
│       ├── LineChart.tsx
│       ├── BarChart.tsx
│       └── ...
├── styles/
│   ├── tokens.ts     # Design tokens
│   ├── globals.css   # Global styles
│   └── utilities.ts  # Utility functions
└── hooks/            # Custom hooks
    ├── useTheme.ts
    ├── useToast.ts
    └── ...