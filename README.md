{
      headerName: 'CIK',
      filter: 'agTextColumnFilter',
      hide: true,
      field: 'sub_cik',
      width: 100,
      cellRenderer: ToolTipComponent,
      cellRendererParams: (params:any) => {
        const formattedValue = params.data.sub_cik.toLocaleString();
        return {
          content: formattedValue,
          type: { 
            value: formattedValue,
            toolTip: {
              value: `Central Index Key: ${formattedValue}`
            }
          }
        }
      }
    },
    {
      headerName: 'Additional CIKs',
      hide: true,
      field: 'sub_aciks',
      filter: 'agTextColumnFilter',
      width: 200,
      cellRenderer: ToolTipComponent,
      cellRendererParams: (params:any) => {
        const formattedValue = params.data.sub_aciks
        .map((cik:number) => cik.toLocaleString())
        .join(', ');

        return {
          content: formattedValue,
          type: { 
            value: formattedValue,
            toolTip: {
              value: `Additional Central Index Key: ${formattedValue}`
            }
          }
        }
      }
    },

<mat-form-field appearance="outline" [class]="class">
      <mat-label>{{ label }}</mat-label>
      @if (group) {
        <mat-select
          [disabled]="disabled"
          (selectionChange)="onChange(select.value); updateVal(select.value)"
          #select
          [multiple]="multiple"
          [value]="_value"
          [formControl]="itemVal"
          [class]="class"
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


import {
  Component,
  Input,
  Output,
  EventEmitter,
  forwardRef,
  OnInit,
  ViewChild,
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
export class SelectComponent implements OnInit, ControlValueAccessor {
  @Input() disabled = false;
  @Input() label!:string;
  @Input() items = <any>[];
  @Input() multiple = false;
  @Input() class!:string;
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

  public ngOnInit(): void {
    console.log("Current Class: ", this.class);  
  }

  @Output() onselect = new EventEmitter<any>();
  @Output() resetGrid = new EventEmitter<any>();

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
