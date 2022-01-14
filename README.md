# vue-self-adaption-view
> self-adaption-view

## Installation

``` bash
npm install vue-self-adaption-view
```

###Using

#### import
``` bash
import SelfAdaptionView from 'vue-self-adaption-view'
export default {
    name: 'app',
    components: {
        SelfAdaptionView
    },
    data() {
        return {
            list: [1, 2, 3, 4, 5, 6, 7],
            minWidth: 300,
            width: 144,
            paginationColor: 'red',
        }
    },
}
```
#### Use in template
``` bash
<SelfAdaptionView
    :list="list"
    :minWidth="minWidth"
    :paginationColor="paginationColor"
    :width="width">
    <template slot="source" slot-scope="{ source }">
        
    </template>
</SelfAdaptionView>
```
#### Options

| Name            |              Description          |  type   |  default  | require |
| :----------     | :-------------------------------- |  :----: |  :------: |  :----: |
| backgroundColor | background color                  |  String |  #ffffff  |  false  |
| paginationColor | Page button color                 |  String |  #000000  |  false  |
| list            | data source                       |  Array  |           |  true   |
| width           | the width of a single element     |  Number |           |  true   |
| min-width       | the minimum width of the container|  Number |           |  true   |


