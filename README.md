filing.component.ts:

   filingForm = this.fb.group({
    fiscal_year: new UntypedFormControl([this.currentYear]),
    fiscal_period: new UntypedFormControl(''),
    date_filed: new UntypedFormControl(''),
    date_to: new UntypedFormControl(''),
    sub_form: new UntypedFormControl(''),
    include8k: new UntypedFormControl(''),
    recent: new UntypedFormControl(false),
  });
  
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
    </div>

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
import {
  Component,
  Input,
  Output,
  EventEmitter,
  forwardRef,
  OnInit,
  ViewChild,
  OnChanges,
  SimpleChanges,
} from '@angular/core';
import {
  ControlValueAccessor,
  UntypedFormControl,
  NG_VALUE_ACCESSOR,
} from '@angular/forms';
import { MatSelect } from '@angular/material/select';

@Component({
  selector: 'idap-select',
  styles: [
    `
              /* TODO(mdc-migration): The following rule targets internal classes of form-field that may no longer apply for the MDC version. */
              /* TODO(mdc-migration): The following rule targets internal classes of form-field that may no longer apply for the MDC version. */
              ::ng-deep idap-select .mat-form-field-infix {
                width: auto !important;
                min-width: 154px;
              }
            `,
  ],
  templateUrl: './select.component.html',
  providers: [
    {
      provide: NG_VALUE_ACCESSOR,
      useExisting: forwardRef(() => SelectComponent),
      multi: true,
    },
  ],
})
export class SelectComponent implements OnInit, OnChanges, ControlValueAccessor {
  @Input() disabled = false;
  @Input() label = '';
  @Input() items = <any>[];
  @Input() multiple = false;
  @Input('class') customClass!:string;
  @Input() group = false;
  @Input() resetsGridOnValueChange: boolean = false;
  @Input() isFormattedMultiSelectDisplay: boolean = false;
  @ViewChild('select') select!: MatSelect;
  @Input() set selectAllIsDefault(value: boolean) {
    if (value) {
      setTimeout(() => {
        this.select.open();
        this.toggleSelections(true);
        this.select.close();
      }, 0);
    }
  }

  _value: any = null;
  itemVal = new UntypedFormControl('');
  selectAll = false;
  displayTextForMatTrigger = '';
  selectedItemsTooltipText: string = '';

  @Output() onselect = new EventEmitter<any>();
  @Output() resetGrid = new EventEmitter<any>();

  ngOnChanges(changes: SimpleChanges): void { }

  ngOnInit(): void { }

  updateVal(val: any) {
    this.getItemDisplayStringForSelectedValue(val);
    if(this.resetsGridOnValueChange){
      this.resetGrid.emit({resetGrid:true, value: val});
    }
    
    //sometimes string value length matches items length
    // and incorrectly triggers the selectAll
    if (val && typeof val !== 'string' && val.length === this.items.length) this.selectAll = true;
    else this.selectAll = false;
  }
  onChange = (val: any) => {
    this.onselect.emit(val);
  };
  // eslint-disable-next-line @typescript-eslint/no-empty-function
  onTouched = () => {};

  onKeyUp(value: any) {
    //this.onkeyup.emit(value);
    //this.writeValue(value);
  }

  public get value(): any {
    return this._value;
  }

  public set value(v) {

    //Reset checkboxes for 'form' field to checked if select all was submitted
    if(this.selectAll && this.label === 'Form'){
      setTimeout(() => {
        this.select.open();
        this.toggleSelections(true);
        this.select.close();
      }, 0);
    }

    this._value = v;
    this.itemVal.setValue(this._value);
    this.updateVal(v)
  }

  public clear() {
    this.value = '';
    this.itemVal.setValue('');
  }

  writeValue(s: any): void {
    this.value = s ?? '';
    this.onChange?.(this.value);
  }
  registerOnChange(fn: (val: any) => void): void {
    this.onChange = fn;
  }
  registerOnTouched(fn: () => void): void {
    this.onTouched = fn;
  }

  toggleSelections(toggle: any): void {
    this.selectAll = toggle;
    //update UI to check all boxes, but submit blank value to server
    if(toggle && this.label === 'Form'){
      this._value = this.items.map((item: any) => item.value);
      this.itemVal.setValue(this._value);
      this.onChange([]);

    } else if (toggle && this.label !== 'Form') {
      this._value = this.items.map((item: any) => item.value);
      this.itemVal.setValue(this._value);
      this.onChange(this._value);
    } else {
      this._value = <any>[];
      this.itemVal.setValue(this._value);
      this.onChange(this._value);
    }
  }

  //Throws a afterViewCheckedError after the form is saved.
  // but error doesn't cause any issues, currently no other work around - TM
  getItemDisplayStringForSelectedValue(selectedValue: any) {
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
}


filing-component.component.html: 
<div role="group" [attr.aria-labelledby]="headerID">

  <div class="filter-group-header">
    <div>
      <h4 [attr.id]="headerID">{{ tabName === 'fact' ? 'Data' : 'Filings' }}</h4>
    </div>
    <a mat-stroked-button
      (click)="clear()"
      matTooltip="Reset Filings filters to their default.">
      Reset
    </a>
  </div>
  <form [formGroup]="filingForm">
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
    </div>
    <div class="flex justify-between">
      <mat-slide-toggle
        class="mb-16"
        (change)="toggleContent()"
        matTooltip="{{
          showMore
            ? 'Click to minimize more search options'
            : 'Expand for more search options'
        }}"
      >See {{ showMore ? 'less' : 'more' }}</mat-slide-toggle
      >
    </div>
    @if (showMore) {
      <div class="flex">
        <idap-datepicker
          formControlName="date_filed"
          label="Filed"
          hint="MM-DD-YYYY"
          matTooltip="Filing Date Range From"
          class="mr-8"
          #date_filed
          [disabled]="searchingRecent()"
        ></idap-datepicker>
        <idap-datepicker
          formControlName="date_to"
          label="To"
          hint="MM-DD-YYYY"
          matTooltip="Filing Date Range To"
          #date_to
          [disabled]="searchingRecent()"
        ></idap-datepicker>
      </div>
      <div class="flex flex-items-center">
        <idap-select
          label="Form"
          placeholder="Form"
          formControlName="sub_form"
          [items]="forms"
          [multiple]="true"
          [selectAllIsDefault]="true"
          [isFormattedMultiSelectDisplay]="true"
          #sub_form
          class="m-r-4"
          >
        </idap-select>
        <idap-checkbox
          formControlName="include8k"
          tooltip="Results include 8-K filings without financial statements"
          class="pb3"
          >Include 8-K Cover Pages</idap-checkbox
          >
        </div>
        @if (tabName === 'filing') {
          <div class="flex">
            <div></div>
            <idap-checkbox
              formControlName="recent"
              tooltip="Returns only the most recent filings for the filer population (overriding any date and period filters)."
              >Most Recent</idap-checkbox
              >
            </div>
          }
        }
      </form>
</div>

filing-component.component.html:8 ERROR Error: Value must be an array in multiple-selection mode.
    at getMatSelectNonArrayValueError (select.mjs:137:10)
    at _MatSelect._setSelectionByValue (select.mjs:799:15)
    at _MatSelect._assignValue (select.mjs:849:14)
    at _MatSelect.writeValue (select.mjs:620:10)
    at onChange (forms.mjs:3373:23)
    at forms.mjs:3871:42
    at Array.forEach (<anonymous>)
    at FormControl2.setValue (forms.mjs:3871:22)
    at set value (select.component.ts:118:18)
    at _SelectComponent.writeValue (select.component.ts:129:10)
