# ZTSelect

* This is a dropdown/select list style input element
* Possibility to filter the list of elements with a text search

#### Usage
```
<ZTSelect
    :options="[
        {label:'Cape Town', value:'Cape Town'},
        {label:'New York', value:'New York'},
        {label:'London', value:'London'},
        {label:'Chicago', value:'Chicago'},
        {label:'Paris', value:'Paris'},
        {label:'Vancouver', value:'Vancouver'},
        {label:'Melbourne', value:'Melbourne'},
        {label:'Tokyo', value:'Tokyo'},
    ]"
/>
```
#### TODO
* Add emits for value selected and search input
* Add some more options for styling
* Implement "clearable" option
* Show currently selected item when opening dropdown
* Allow option to disable search
* Add slots
