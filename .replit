import csv
from abc import ABC, abstractmethod
import os


class Reminder(ABC):
    @abstractmethod
    def send(self):
        pass


class EmailReminder(Reminder):
    def _init_(self, recipient, message):
        self.recipient = recipient
        self.message = message

    def send(self):
        print(f"Sending email to {self.recipient}: {self.message}")


class SMSReminder(Reminder):
    def _init_(self, phone_number, message):
        self.phone_number = phone_number
        self.message = message

    def send(self):
        print(f"Sending SMS to {self.phone_number}: {self.message}")


class AudioReminder(Reminder):
    def _init_(self, audio_file):
        self.audio_file = audio_file

    def send(self):
        print(f"Playing audio: {self.audio_file}")


class ReminderFactory:
    @staticmethod
    def create_reminder(reminder_type, *args, **kwargs):
        if reminder_type == "email":
            return EmailReminder(*args, **kwargs)
        elif reminder_type == "sms":
            return SMSReminder(*args, **kwargs)
        elif reminder_type == "audio":
            return AudioReminder(*args, **kwargs)
        else:
            raise ValueError("Invalid reminder type")


class ReminderManager:
    def _init_(self, file_path):
        self.file_path = file_path

    def save_reminder(self, reminder_type, *args, **kwargs):
        reminder = ReminderFactory.create_reminder(reminder_type, *args, **kwargs)
        with open(self.file_path, "a", newline="") as file:
            writer = csv.writer(file)
            writer.writerow([reminder_type, *args])

    def get_reminders(self):
        reminders = []
        with open(self.file_path, "r", newline="") as file:
            reader = csv.reader(file)
            for row in reader:
                reminder_type = row[0]
                if reminder_type == "email":
                    reminder = EmailReminder(*row[1:])
                elif reminder_type == "sms":
                    reminder = SMSReminder(*row[1:])
                elif reminder_type == "audio":
                    reminder = AudioReminder(*row[1:])
                else:
                    raise ValueError("Invalid reminder type")
                reminders.append(reminder)
        return reminders


if _name_ == "_main_":
    current_directory = os.path.dirname(os.path.realpath(_file_))
    file_path = os.path.join(current_directory, "reminders.csv")
    manager = ReminderManager(file_path)

    manager.save_reminder("email", "chantelchaps@gmail.com", "Happy Birthday!")
    manager.save_reminder("sms", "+370985767438", "Happy Birthday!")
    manager.save_reminder("audio", "birthday_song.mp3")

    reminders = manager.get_reminders()
    for reminder in reminders:
        reminder.send()