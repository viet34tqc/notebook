# Styled Components Best Practices

## Import styled components as object
- Each component has a folder, including a js file for the component layout and a js file for the styles
- In the component:
```javascript
import * as S from './styles';
<S.Headline>{title}</S.Headline>
```