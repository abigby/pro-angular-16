filing-component.component.html:8 ERROR TypeError: selectedValue.forEach is not a function
    at _SelectComponent.getItemDisplayStringForSelectedValue (select.component.ts:173:21)

    if (selectedValue && this.isFormattedMultiSelectDisplay) {
      let seletectItems: any = [];
      let selectedItemsTooltipText: any = [];
      selectedValue.forEach((value: any) => {
        let item = this.items.find((item: any) => item.value === value);
        seletectItems.push(item);
      });
      if (selectedValue.length > 1 && this.isFormattedMultiSelectDisplay && selectedValue.length !== this.items.length) {
        this.displayTextForMatTrigger = seletectItems[0].viewValue;
        seletectItems.forEach((value: any) => {
          selectedItemsTooltipText.push(value.viewValue);
        });
        this.selectedItemsTooltipText = String(
          selectedItemsTooltipText
        ).replace(/,/g, ', ');
        return this.displayTextForMatTrigger
      } else {
        this.displayTextForMatTrigger = '';
        return this.displayTextForMatTrigger = '';
       } 
    }
    return this.displayTextForMatTrigger = '';
  }
