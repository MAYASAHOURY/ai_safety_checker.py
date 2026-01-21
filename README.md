# ai_safety_checker.py
  "A collection of Python scripts designed to evaluate LLM outputs for security vulnerabilities and ensure AI-generated code follows safety best practices (Red Teaming approach)."
import re

def check_ai_code_safety(code_snippet):
    """
    سكربت لفحص سلامة الكود البرمجي الذي يولده الذكاء الاصطناعي
    Checks for common vulnerabilities like SQL Injection.
    """
    vulnerabilities = []
    
    # البحث عن ثغرة SQL Injection (استخدام f-strings في الاستعلامات)
    sql_pattern = r"execute\(f?['\"].*SELECT.*WHERE.*\{.*\}['\"]\)"
    
    if re.search(sql_pattern, code_snippet, re.IGNORECASE):
        vulnerabilities.append("CRITICAL: Potential SQL Injection vulnerability detected. Use parameterized queries instead.")

    # البحث عن كلمات سر مسربة في الكود
    hardcoded_secrets = r"(api_key|password|secret|token)\s*=\s*['\"].+['\"]"
    if re.search(hardcoded_secrets, code_snippet, re.IGNORECASE):
        vulnerabilities.append("WARNING: Hardcoded credentials/secrets found in code.")

    return vulnerabilities

# تجربة السكربت على كود "مشبوه"
test_code = """
query = f"SELECT * FROM users WHERE username = '{user_input}'"
db.execute(query)
"""

print("--- AI Code Safety Analysis ---")
results = check_ai_code_safety(test_code)

if results:
    for issue in results:
        print(f"[!] {issue}")
else:
    print("[+] Code appears to follow basic safety patterns.")