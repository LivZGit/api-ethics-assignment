Task 1: Classification and Handling of PII Fields

The dataset contains both direct and indirect personally identifiable information. Each field must be handled carefully to protect user privacy while keeping the data useful for research.

full_name is direct PII because it directly identifies a person. It should be pseudonymized so that records can still be tracked without revealing identity.

email is direct PII and uniquely identifies a person. It is not required for analysis, so it should be removed completely.

date_of_birth is indirect PII because it can help identify someone when combined with other data. It should be masked by converting it into age.

zip_code is indirect PII as it reveals location. It should be partially masked to reduce precision, for example by hiding the last digits.

job_title is indirect PII. It can be kept because it is useful for analysis, but it may be generalized to reduce risk.

diagnosis_notes contain sensitive health information and may include identifiers. They should be cleaned to remove any personal references while keeping medical information.

Task 2: Ethical Issues in API Script

The given script has two main ethical issues.

The first issue is that the script makes repeated API requests without any delay or error handling. This can violate the API’s usage limits and terms of service. It may overload the server or lead to the API key being blocked. To fix this, a delay should be added between requests and the response status should be checked.

Corrected code:

'''

import requests
import time

API_URL = "https://healthstats-api.example.com/records"
API_KEY = "free_tier_key_abc123"

records = []

for page in range(1, 101):
response = requests.get(API_URL, params={"page": page, "key": API_KEY})

    if response.status_code == 200:
        data = response.json()
        records.extend(data.get("results", []))
    else:
        break

    time.sleep(1)


'''

The second issue is that the script stores all raw data permanently, including sensitive and personally identifiable information. This goes against the principle of data minimization and increases privacy risks. The data should be cleaned and anonymized before storing.

Corrected code:

'''

def clean_record(record):
return {
"age": calculate_age(record["date_of_birth"]),
"zip_code": record["zip_code"][:3] + "\*\*\*",
"job_title": record["job_title"],
"diagnosis_notes": remove_identifiers(record["diagnosis_notes"])
}

cleaned_records = [clean_record(r) for r in records]

save_to_database(cleaned_records)

'''
