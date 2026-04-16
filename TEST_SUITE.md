# Test Suite – Resume Optimization Feature
Prepared by: Craig Zyrus Manuel  
Position Applied: Software Tester Intern  

---

## Feature Under Test
**Resume Optimization**

This feature allows users to:
- enter a target role
- optionally add a company name
- paste a job description
- import a job description from URL
- generate an ATS-optimized resume

---

## Scope
This test suite covers:
- functional testing
- input validation
- negative testing
- edge cases
- performance
- usability
- security checks

---

## Test Cases

### TC-001 Successful Resume Optimization
**Priority:** High

**Objective:** Verify successful optimization with valid input

**Steps:**
1. Navigate to Resume Optimization
2. Enter target role: `Software Tester Intern`
3. Enter company name: `Dashlabs.ai`
4. Paste a valid job description
5. Click **Optimize**

**Expected Result:**
- loading indicator appears
- optimized resume is generated
- output preview is displayed
- no error message appears

---

### TC-002 Empty Required Fields
**Priority:** High

**Objective:** Validate required field checks

**Steps:**
1. Leave role blank
2. Leave job description blank
3. Click **Optimize**

**Expected Result:**
Validation message appears and optimization does not proceed

---

### TC-003 Optional Company Name
**Priority:** Medium

**Objective:** Verify optional field behavior

**Steps:**
1. Enter role
2. Leave company blank
3. Enter job description
4. Click **Optimize**

**Expected Result:**
Optimization still completes successfully

---

### TC-004 Invalid URL Input
**Priority:** High

**Objective:** Check invalid URL handling

**Steps:**
1. Select import from URL
2. Enter invalid URL format
3. Submit

**Expected Result:**
User-friendly error message appears
Application remains stable

---

### TC-005 Large Input Handling
**Priority:** Medium

**Objective:** Test long job description input

**Input:** 5000+ words

**Expected Result:**
System remains responsive
No truncation or crash

---

### TC-006 XSS / Script Injection
**Priority:** High

**Input:**
`<script>alert('test')</script>`

**Expected Result:**
Input is sanitized
No script execution occurs

---

### TC-007 Network Failure
**Priority:** High

**Objective:** Test behavior during internet interruption

**Steps:**
1. Start optimization
2. Disconnect internet during loading

**Expected Result:**
Graceful error message appears
Retry option is shown

---

### TC-008 Re-Optimization
**Priority:** Medium

**Objective:** Verify repeat optimization

**Steps:**
1. Generate first optimized version
2. Click optimize again

**Expected Result:**
New output is generated
Previous output remains intact unless intentionally replaced

---

## Non-Functional Testing

### Performance
Expected response time: under 10 seconds

### Compatibility
- Chrome
- Firefox
- Edge

### Usability
- clear button labels
- visible loading state
- readable results