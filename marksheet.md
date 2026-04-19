# Lab Marksheet
- **GitHub Username:** curly
- **Passed:** 8
- **Failed:** 0

## Check Results
      echo -e "\033[1;32m✅ PASS:\033[0m $1"
      echo -e "\033[1;31m❌ FAIL:\033[0m $1"
      grep -E "PASS:|FAIL:" "$0" | sed 's/^/    /'
