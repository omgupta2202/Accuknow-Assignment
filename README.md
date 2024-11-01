# Accuknow-Assignment

### Question 1: By default, are Django signals executed synchronously or asynchronously?

Answer: By default, Django signals are executed synchronously. This means that the signal handlers run immediately in the same thread as the caller.

    Proof with Code: To demonstrate that signals are synchronous, we can use timestamps. If signals were asynchronous, the signal handler would execute after a delay, not immediately:

        @receiver(post_save, sender=MyModel)
        def my_signal_handler(sender, instance, **kwargs):
            print(f"Handler timestamp: {time.time()}")
        print(f"Caller timestamp: {time.time()}")
        instance = MyModel.objects.create()

### Question 2: Do Django signals run in the same thread as the caller?

Answer: Yes, Django signals run in the same thread as the caller by default.

    Proof with Code: To prove this, we can print the thread identifier in both the caller and the signal handler. If they match, it shows they’re running in the same thread:

        @receiver(post_save, sender=MyModel)
        def my_signal_handler(sender, instance, **kwargs):
            print(f"Handler thread ID: {threading.get_ident()}")
            print(f"Caller thread ID: {threading.get_ident()}")
            instance = MyModel.objects.create()

### Question 3: By default, do Django signals run in the same database transaction as the caller?

Answer: Yes, Django signals run in the same database transaction as the caller by default, provided that the signal is triggered within a transaction block (like transaction.atomic()).

    Proof with Code: We can test this by checking if a signal handler can detect a rollback in the caller transaction. If a rollback occurs and the signal doesn’t see the data change, it means it’s in the same transaction.

        @receiver(post_save, sender=MyModel)
        def my_signal_handler(sender, instance, **kwargs):
            print("Signal handler sees the instance:", instance)
        try:
            with transaction.atomic():
                instance = MyModel.objects.create()
                print("Caller sees the instance:", instance)
                raise Exception("Forcing rollback")
        except:
            print("Transaction rolled back")

---

## Rectangle Class in Python


    class Rectangle:    
        def __init__(self, length: int, width: int):
            self.length = length
            self.width = width

        def __iter__(self):
            yield {'length': self.length}
            yield {'width': self.width}

# Example
rect = Rectangle(5, 10)
   
    for dimension in rect:
        print(dimension)
