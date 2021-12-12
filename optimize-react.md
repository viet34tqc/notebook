# Optimize React

<https://github.com/twclark0/react-performance>

## Debounce callbacks on DOM events

use `debounce` from lodash library

`import {debounce} from "lodash";`

## use React Window package to virtualize long list

## Tree shaking modules

Instead of importing like above, we should do like this

`import debounce from "lodash/debounce";`