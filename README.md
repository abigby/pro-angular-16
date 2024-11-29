Write unite tests to ensure the menu item renders correctly.
Write integration tests to verify that clicking the menu item routes the user to the My groups home/landing page. 
Conduct cross-browser and responsive testing to confirm the menu item functions correctly on all supported devices and platforms. 


      <mat-sidenav-container [hasBackdrop]="true" class="bg-white">
        <mat-sidenav class="w-25" mode="over" [opened]="ui.sideNavOpen | async">
          <mat-nav-list class="">
            <mat-list-item routerLink="my-profile">My Profile</mat-list-item>
            <mat-list-item routerLink="predef-query">Predefined Queries</mat-list-item>
            <mat-list-item routerLink="xbrl-summary">XBRL Submission Analysis</mat-list-item>
            <mat-list-item routerLink="my-groups">My Groups</mat-list-item>
            <mat-list-item>Save Query</mat-list-item>
            <mat-list-item>Upload Query</mat-list-item>
            <mat-list-item>Fact Notes Report</mat-list-item>
            <mat-divider></mat-divider>
            <h3 matSubheader>Other Applications</h3>
            <mat-list-item>
              <a style="text-decoration: none !important"
                  href="{{ aaqvURL }}"
                  target="_blank"
                  rel="noopener">
                AAQV
              </a></mat-list-item>
            <mat-list-item>
              <a style="text-decoration: none !important"
                        href="{{ iViewURL }}"
                        target="_blank"
                        rel="noopener">
                iView
              </a>
            </mat-list-item>
            <mat-list-item>
              <a style="text-decoration: none !important"
                        href="{{ fpURL }}"
                        target="_blank"
                        rel="noopener">
                Filer Profile
                </a>
            </mat-list-item>
            <mat-list-item>
              <a style="text-decoration: none !important"
                        href="{{ usGaapURL }}"
                        target="_blank"
                        rel="noopener">
                US-GAAP Viewer
              </a>
            </mat-list-item>
    </mat-nav-list>
  </mat-sidenav>
   <mat-sidenav-content>
     <router-outlet></router-outlet>
  </mat-sidenav-content>
</mat-sidenav-container>
