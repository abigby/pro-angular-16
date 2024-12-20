import { HttpClient } from '@angular/common/http';
import { Injectable } from '@angular/core';
import { BehaviorSubject, Observable, share, tap } from 'rxjs';
import { environment } from '../../../../../environments/environment';

@Injectable({
  providedIn: 'root',
})
export class SearchFilersService {

  public _filersSubFormattedForDisplayUse = new BehaviorSubject<any>(null);

  public filersSubFormattedForDisplayUse$: Observable<any> = this._filersSubFormattedForDisplayUse;

  public filerNames: any;
  public filerTickers: any;
  public filerCIKs: any;

  private baseUrl = environment.baseUrl;
  private userDetailsURL = `${this.baseUrl}/fsqv/api/rest/getUserDetails`;
  constructor(private http: HttpClient) {
    //call API when service instance is created
    this.getFilers().subscribe();
  }

  public getFilers() {
    return this.http
      .get(
        `${environment.baseUrl}/fsqv/api/rest/searchFilers?input=`
      )
      .pipe(
        tap((value)=>{
          this._filersSubFormattedForDisplayUse.next(this.formatFilers(value));
        }
        
      ));
  }

  public getCountries() {
    return this.http.get(
      `${environment.baseUrl}/fsqv/api/rest/countryCodesAndNames`
    );
  }

  public getSectors() {
    return this.http.get(
      `${environment.baseUrl}/fsqv/api/rest/sector?input=`
    );
  }
  public getIndustries() {
    return this.http.get(
      `${environment.baseUrl}/fsqv/api/rest/industry?input=`
    );
  }
  public getOffices() {
    return this.http.get(
      `${environment.baseUrl}/fsqv/api/rest/office?input=`
    );
  }
  public getAutoLogin() {
    return this.http.get(
      `${environment.baseUrl}/fsqv/api/rest/autoLogin`
    );
  }
  //POST needed an empty object ot work
  public getManualLogin(username: string, password: string) {
    return this.http.post(
      `${environment.baseUrl}/fsqv/api/rest/login/` +
        username +
        '/' +
        password,
      {}
    );
  }

  public getCustomGroups(userId: string) {
    return this.http.get(
      `${environment.baseUrl}/fsqv/api/rest/customGroup?userId=` +
        userId
    );
  }

  public getUserDetails() {
    console.log('BannerService : getUserDetails...');
    return this.http.get<any>(`${this.userDetailsURL}`);
  }
  
  public getQueryParams(id: string, userId: string){
    return this.http.get(
      `${environment.baseUrl}/fsqv/api/rest/sq/getSavedQuery?id=`+id+'&userId='+userId
    );
  }

  public formatFilers(filers: any){
    
    this.filerNames = filers.map((f: any) => {
                return {
                  id: f.cik,
                  searchName: f.name,
                  displayName: `${f.name}(${f.cik})`,
                };
              });
    this.filerTickers = filers.map((f: any) => {
       return {
           id: f.cik,
            searchName: f.ticker,
            displayName: `${f.ticker}(${f.cik})`,
            };
         });

     this.filerCIKs = filers.map((f: any) => {
          return {
            id: f.cik,
            searchName: f.cik,
            displayName: `${f.cik}`,
          };
        });

      let filersSubFormattedForDisplayUse = {
        filerNames: this.filerNames,
        filerTickers: this.filerTickers,
        filerCIKs: this.filerCIKs
      };

      return filersSubFormattedForDisplayUse;

     
  }
}



<h2 mat-dialog-title>Add Custom Group</h2>
<mat-dialog-content>
    <form>
        <div class="mb-16">
            <mat-form-field appearance="outline" floatLabel="always" hideRequiredMarker="true" class="w-100">
                <mat-label>Group Name <i>(required)</i></mat-label>
                <input
                matInput
                required>
                <mat-hint>Only numbers, letters, "-" and "_" allowed. Minimum of 2 and maximum of 50 character.</mat-hint>
                <mat-error>Group Name is required.</mat-error>
                <!-- @if (groupNameInput.invalid) {
                <mat-error>{{errorMessage()}}</mat-error>
                } -->
            </mat-form-field>
        </div>
        <div class="flex flex-justify-between">
            <mat-form-field appearance="outline" floatLabel="always" class="mr-8">
                <mat-label>Type</mat-label>
                <mat-select>
                    <mat-option value="name">Name</mat-option>
                    <mat-option value="cik">CIK</mat-option>
                    <mat-option value="ticker">Ticker</mat-option>
                </mat-select>
            </mat-form-field>

            <mat-form-field appearance="outline" floatLabel="always" class="w-100">
                <mat-label>Filer</mat-label>
                <input type="text"
                        aria-label="Filer"
                        matInput
                        [formControl]="myControl"
                        [matAutocomplete]="auto">
                <mat-autocomplete #auto="matAutocomplete">
                    @for (option of getFilersService.filersSubFormattedForDisplayUse$ | async; track option) {
                        <mat-option [value]="option.cik">{{option.name}}</mat-option>
                    }
                </mat-autocomplete>
                <mat-hint>Start typing to search filers. Select a filer to add it to the table below.</mat-hint>
            </mat-form-field>
        </div>
    </form>

    <div class="dialogTableContainer table-wrapper mt-16">
        <app-groups-filers-table></app-groups-filers-table>
    </div>

</mat-dialog-content>
<mat-dialog-actions align="end">
  <button mat-stroked-button class="mr-auto" mat-dialog-close>Cancel</button>
  <button mat-flat-button color="primary" [mat-dialog-close]="true" cdkFocusInitial>Add Group</button>
</mat-dialog-actions>


import { AsyncPipe } from "@angular/common";
import { OnInit, OnChanges, Input, SimpleChanges, Component } from "@angular/core";
import { ReactiveFormsModule, FormsModule, FormControl, UntypedFormGroup, UntypedFormBuilder, UntypedFormControl } from "@angular/forms";
import { MatAutocompleteModule } from "@angular/material/autocomplete";
import { MatButtonModule } from "@angular/material/button";
import { MatDialogModule } from "@angular/material/dialog";
import { MatFormFieldModule } from "@angular/material/form-field";
import { MatIconModule } from "@angular/material/icon";
import { MatInputModule } from "@angular/material/input";
import { MatSelectModule } from "@angular/material/select";
import { MatTooltipModule } from "@angular/material/tooltip";
import { GroupsFilersTableComponent } from "../../groups-filers-table/groups-filers-table.component";
import { SearchFilersService } from "src/app/shared/data-access/static-data";

@Component({
    selector: 'add-group-dialog-content',
    templateUrl: './add-group-dialog-content.component.html',
    styleUrl: './add-group-dialog-content.component.scss',
    standalone: true,
    imports: [
      MatDialogModule,
      MatButtonModule,
      MatFormFieldModule,
      MatInputModule,
      MatSelectModule,
      ReactiveFormsModule,
      MatAutocompleteModule,
      FormsModule,
      AsyncPipe,
      MatIconModule,
      MatTooltipModule,
      GroupsFilersTableComponent
    ],
  })
  export class AddGroupDialogContent implements OnInit, OnChanges {
    @Input() 
    public filerSubFormatted!: any;
  
    public myControl: FormControl<string | null> = new FormControl('');
    public options: Array<string> = ['One', 'Two', 'Three'];
    public filerNames:Array<any> = new Array<any>();
    public selectedList: Array<any> = new Array<any>();
  
    public filerForm: UntypedFormGroup = new UntypedFormGroup({});
  
    constructor(
      private fb: UntypedFormBuilder,
      public getFilersService: SearchFilersService,
    ) {}
  
    public ngOnChanges(changes: SimpleChanges): void {
      if(changes) {
        if(changes['filerSubFormatted']?.currentValue) {
          this.filerNames = this.filerSubFormatted.filerNames;
          this.selectedList = this.filerNames;
        }
      }
    }
    public ngOnInit(): void {
      this.filerForm = this.fb.group({
        searchType: new UntypedFormControl('Name'),
        filers: new UntypedFormControl(''),
        groupName: new UntypedFormControl('Group Name')
      });

    }
  
    public clearForm(): void {
      this.filerForm.reset();
      this.filerForm.get('searchType')?.patchValue('Name');
    }
  
  }
