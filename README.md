# Afteryourselfimport os
from datetime import datetime

# Custom Exception
class DuplicateVisitorError(Exception):
    """Raised when the visitor is the same as the last recorded one."""
    pass


def log_visitor(filename="visitors.txt"):
    try:
        visitor_name = input("Enter visitor's name: ").strip()

        last_name = None

        # Check if file exists and read last line
        if os.path.exists(filename):
            with open(filename, "r") as file:
                lines = file.readlines()
                if lines:
                    last_line = lines[-1].strip()
                    if " - " in last_line:  # expected format: name - timestamp
                        last_name = last_line.split(" - ")[0]

        # Raise error if last visitor is the same
        if visitor_name == last_name:
            raise DuplicateVisitorError(f"Duplicate visitor detected: {visitor_name}")

        # Add new visitor with timestamp
        timestamp = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
        with open(filename, "a") as file:
            file.write(f"{visitor_name} - {timestamp}\n")

        print(f"Visitor '{visitor_name}' logged successfully!")

    except DuplicateVisitorError as e:
        print("Error:", e)
    except Exception as e:
        print("An unexpected error occurred:", e)


if __name__ == "__main__":
    log_visitor()
