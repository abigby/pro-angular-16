filing.component.ts:
 
  public reset() {
    this.filingForm.reset();
  }

  getYears() {
    this.years = Array.from({ length: parseInt(this.currentYear) - 2008 + 1 }, (_, index) => {
      const year = parseInt(this.currentYear) - index;
      return {
        viewValue: year.toString(),
        value: year.toString()
      };
    });
  }


filing.component.html:
<div class="flex flex-justify-between">
      <idap-select
        [items]="years"
        [isFormattedMultiSelectDisplay]="true"
        formControlName="fiscal_year"
        label="Year"
        [multiple]="true"
        [class]="'w-100'"
        class="w-100 mr-8"
      ></idap-select>
      <idap-select
        [items]="periods"
        [isFormattedMultiSelectDisplay]="true"
        [selectAllIsDefault]="true"
        formControlName="fiscal_period"
        [class]="'w-100'"
        class="w-100"
        label="Period"
        [multiple]="true"
      ></idap-select>
    </div>filing-component.component.html:8 ERROR TypeError: selectedValue.forEach is not a function
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

select.component.html: 
<mat-form-field appearance="outline" [class]="customClass">
      <mat-label>{{ label }}</mat-label>
      @if (group) {
        <mat-select
          [disabled]="disabled"
          (selectionChange)="onChange(select.value); updateVal(select.value)"
          #select
          [multiple]="multiple"
          [value]="_value"
          [formControl]="itemVal"
          [class]="customClass"
          >
          @if (multiple) {
            <mat-checkbox
              [checked]="selectAll"
              class="mat-option"
              (change)="toggleSelections($event.checked)"
              >
              Select All
            </mat-checkbox>
          }
          <!-- <mat-option value="">All</mat-option> -->
          @for (group of items; track group) {
            <mat-optgroup [label]="group.name">
              @for (item of group.list; track item) {
                <mat-option
                  [value]="item.value || item"
                  [matTooltip]="item.toolTip"
                  matTooltipPosition="after"
                  matTooltipClass="idap-tooltip"
                  (change)="toggleSelections(item.viewValue)"
                  >
                  {{ item.viewValue || item }}
                </mat-option>
              }
            </mat-optgroup>
          }
        </mat-select>
      }
      @if (!group) {
        <mat-select
          (selectionChange)="onChange(select.value); updateVal(select.value)"
          #select
          [multiple]="multiple"
          [value]="_value"
          [formControl]="itemVal"
          matTooltip="{{ displayTextForMatTrigger !== '' ? this.selectedItemsTooltipText : null }}"
          matTooltipClass="idap-tooltip"
          >
          @if (multiple) {
            <mat-checkbox
              [checked]="selectAll"
              class="mat-option"
              (change)="toggleSelections($event.checked); getItemDisplayStringForSelectedValue(null)"
              >
              Select All
            </mat-checkbox>
          }
          @if (selectAll) {
            <mat-select-trigger>All</mat-select-trigger>
          }
          @if (
            select.value?.length > 1 &&
            !selectAll &&
            isFormattedMultiSelectDisplay
            ) {
            <mat-select-trigger
              >
              {{ getItemDisplayStringForSelectedValue(select.value) }}
              <span>
                (+{{ select.value.length - 1 }}
                {{ select.value?.length === 2 ? 'other' : 'others' }})
              </span>
            </mat-select-trigger>
          }
          @for (item of items; track item) {
            <mat-option [value]="item.value || item">
              {{ item.viewValue || item }}
            </mat-option>
          }
        </mat-select>
      }
    </mat-form-field>

select.component.ts: 
public set value(v) {

    //Reset checkboxes for 'form' field to checked if select all was submitted
    if(this.selectAll && this.label === 'Form'){
      setTimeout(() => {
        this.select.open();
        this.toggleSelections(true);
        this.select.close();
      }, 0);
    }

    
    // if(this.label.toLowerCase() === "year") {
    //   this._value = [v];
    // } else {
      this._value = v;
    // }

    this.itemVal.setValue(this._value);
    this.updateVal(this._value);
  }
