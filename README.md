
// menu-item.component.spec.ts

import { ComponentFixture, TestBed } from '@angular/core/testing';
import { MatSidenavModule } from '@angular/material/sidenav';
import { MatListModule } from '@angular/material/list';
import { RouterTestingModule } from '@angular/router/testing';
import { MenuComponent } from './menu.component';

describe('MenuComponent', () => {
  let component: MenuComponent;
  let fixture: ComponentFixture<MenuComponent>;

  beforeEach(async () => {
    await TestBed.configureTestingModule({
      declarations: [MenuComponent],
      imports: [MatSidenavModule, MatListModule, RouterTestingModule]
    }).compileComponents();

    fixture = TestBed.createComponent(MenuComponent);
    component = fixture.componentInstance;
    fixture.detectChanges();
  });

  it('should render the "My Groups" menu item', () => {
    const compiled = fixture.nativeElement;
    const menuItem = compiled.querySelector('mat-list-item[routerLink="my-groups"]');
    expect(menuItem).toBeTruthy();
    expect(menuItem.textContent.trim()).toBe('My Groups');
  });
});


// menu-item-routing.spec.ts

import { TestBed } from '@angular/core/testing';
import { RouterTestingModule } from '@angular/router/testing';
import { Router } from '@angular/router';
import { Location } from '@angular/common';
import { MenuComponent } from './menu.component';
import { MyGroupsComponent } from '../my-groups/my-groups.component';
import { MatSidenavModule } from '@angular/material/sidenav';
import { MatListModule } from '@angular/material/list';

describe('Menu Routing', () => {
  let router: Router;
  let location: Location;

  beforeEach(async () => {
    await TestBed.configureTestingModule({
      declarations: [MenuComponent, MyGroupsComponent],
      imports: [
        RouterTestingModule.withRoutes([
          { path: 'my-groups', component: MyGroupsComponent }
        ]),
        MatSidenavModule,
        MatListModule
      ]
    }).compileComponents();

    router = TestBed.inject(Router);
    location = TestBed.inject(Location);
    router.initialNavigation();
  });

  it('should navigate to "My Groups" when menu item is clicked', async () => {
    const fixture = TestBed.createComponent(MenuComponent);
    const compiled = fixture.nativeElement;
    const menuItem = compiled.querySelector('mat-list-item[routerLink="my-groups"]');

    menuItem.click();
    await fixture.whenStable(); // Wait for the router to update.

    expect(location.path()).toBe('/my-groups');
  });
});
