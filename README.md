#PASSWORD GENERATOR
A **Smart Password Generator** is a utility tool designed to enhance digital security by creating complex, unpredictable strings of characters that are resistant to "brute-force" attacks. By leveraging Python's `random` and `string` modules, the program assembles passwords using a custom blend of uppercase letters, lowercase letters, numbers, and symbols based on specific user requirements. Beyond simple generation, this tool incorporates a **Strength Evaluator** that utilizes regular expressions to analyze entropy and complexity, providing real-time feedback on the password's security level to ensure it meets modern safety standards.

#CODE
import string
import random
import re
import sys

def generate_password(length, use_digits=True, use_special=True):
    # Base characters: Always include letters
    chars = string.ascii_letters 
    if use_digits:
        chars += string.digits
    if use_special:
        chars += string.punctuation

    # Safety check: if somehow length is too small
    if length < 4:
        length = 4
        
    password = ''.join(random.choice(chars) for _ in range(length))
    return password

def check_strength(password):
    score = 0
    
    # Criteria check
    if len(password) >= 12: score += 2
    elif len(password) >= 8: score += 1
    
    if re.search(r"\d", password): score += 1
    if re.search(r"[a-z]", password) and re.search(r"[A-Z]", password): score += 1
    if re.search(r"[!@#$%^&*(),.?\":{}|<> ]", password): score += 1

    if score <= 2: return "Weak 🔴", score
    elif score <= 4: return "Moderate 🟡", score
    return "Strong 🟢", score

def main():
    print("\n--- SMART PASSWORD GENERATOR ---")
    try:
        # Get inputs
        val = input("Enter password length (min 8): ")
        pwd_len = int(val)
        
        include_nums = input("Include numbers? (y/n): ").lower() == 'y'
        include_special = input("Include special characters? (y/n): ").lower() == 'y'
        
        # Process
        result = generate_password(pwd_len, include_nums, include_special)
        label, points = check_strength(result)
        
        # Display
        print("\n" + "="*30)
        print(f"YOUR PASSWORD: {result}")
        print(f"STRENGTH:      {label} ({points}/5)")
        print("="*30 + "\n")
        
    except ValueError:
        print("❌ Error: Please enter a valid number for length.")
    except Exception as e:
        print(f"❌ An unexpected error occurred: {e}")

if __name__ == "__main__":
    main()
