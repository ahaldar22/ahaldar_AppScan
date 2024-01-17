1. AppScan EU URL
https://cloud.appscan.com/eu/

2. AltoroJ on Github link
https://github.com/HCL-TECH-SOFTWARE/AltoroJ

3.  AppScan SAST GitHub YAML
```
name: "HCL AppScan SAST"
on:
  workflow_dispatch
jobs:
  scan:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v3
      - name: Run AppScan SAST Scan
        uses: HCL-TECH-SOFTWARE/appscan-sast-action@v1.0.1
        with:
          baseurl:  https://cloud.appscan.com/eu/
          asoc_key: ${{secrets.ASOC_KEY}}
          asoc_secret: ${{secrets.ASOC_SECRET}}
          application_id: ${{vars.ASOC_APPID_SAST}}
```
4. AppScan DAST GitHub YAML 
```
name: "HCL AppScan DAST - basic"
on:
  workflow_dispatch
jobs:
  scan:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v3
      - name: Run ASoC DAST Scan
        uses: HCL-TECH-SOFTWARE/appscan-dast-action@v1.0.5
        with:
          baseurl:  https://cloud.appscan.com/eu/
          asoc_key: ${{secrets.ASOC_KEY}}
          asoc_secret: ${{secrets.ASOC_SECRET}}
          application_id: ${{vars.ASOC_APPID_DAST}}
          scan_type: staging
          dynamic_scan_type: dast
          starting_URL: 'https://demo.testfire.net?mode=demo'
          login_method: userpass
          login_user: jsmith
          login_password: demo1234
          network: public
          fail_for_noncompliance: false
          wait_for_analysis: true
      - uses: actions/upload-artifact@v3
        name: Upload HCL AppScan HTML Report to Github Artifacts
        with:
          name: AppScan Security Scan HTML Report 
          path: '**/AppScan*.html'
        if: success() || failure()
```