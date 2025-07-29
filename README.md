import datetime

class PulmoCare:
    def __init__(self):
        self.records = []

    def log_breathing_rate(self, breaths_per_minute):
        timestamp = datetime.datetime.now()
        self.records.append((timestamp, breaths_per_minute))
        print(f"Logged: {breaths_per_minute} breaths/min at {timestamp.strftime('%Y-%m-%d %H:%M:%S')}")

    def average_breathing_rate(self):
        if not self.records:
            return 0
        total = sum(rate for _, rate in self.records)
        return total / len(self.records)

    def check_abnormal(self):
        abnormal_records = []
        for timestamp, rate in self.records:
            if rate < 12:
                abnormal_records.append((timestamp, rate, "Below normal (bradypnea)"))
            elif rate > 20:
                abnormal_records.append((timestamp, rate, "Above normal (tachypnea)"))
        return abnormal_records

    def show_report(self):
        avg = self.average_breathing_rate()
        print(f"\nAverage Breathing Rate: {avg:.2f} breaths per minute\n")
        abnormal = self.check_abnormal()
        if abnormal:
            print("Abnormal readings detected:")
            for time, rate, condition in abnormal:
                print(f" - {time.strftime('%Y-%m-%d %H:%M:%S')}: {rate} bpm ({condition})")
        else:
            print("No abnormal readings detected.")

def main():
    app = PulmoCare()

    print("Welcome to Pulmo Care - Respiratory Rate Tracker")
    print("Enter breathing rates (breaths per minute). Type 'q' to quit and show report.")

    while True:
        entry = input("Breathing rate: ")
        if entry.lower() == 'q':
            break
        try:
            rate = int(entry)
            app.log_breathing_rate(rate)
        except ValueError:
            print("Please enter a valid integer or 'q' to quit.")

    app.show_report()

if __name__ == "__main__":
    main()
