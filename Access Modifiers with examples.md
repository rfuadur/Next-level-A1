# Access Modifiers in TypeScript: public, private, protected and readonly

Access modifiers decide who is allowed to use a property or a method of a class. TypeScript gives us four of them. They let us hide the parts of a class that other code should not touch.

`public` is the default. Anyone can read it and change it. `private` means only the same class can use it, not even a child class. `protected` is a little more open, because the class and its child classes can use it, but code outside cannot. `readonly` is different from the other three. It does not hide anything. It only stops a value from being changed after it is set.

Here is a class that uses all four:

```ts
class BankAccount {
  public readonly accountNumber: string;
  protected balance: number;
  private pin: string;

  constructor(accountNumber: string, balance: number, pin: string) {
    this.accountNumber = accountNumber;
    this.balance = balance;
    this.pin = pin;
  }

  public verify(input: string): boolean {
    return this.pin === input;
  }
}
```

The pin never leaves the class. Other code can only call `verify` and get true or false.

A child class can still use `balance`, because it is protected:

```ts
class SavingsAccount extends BankAccount {
  public addInterest(rate: number): number {
    this.balance += this.balance * rate;
    return this.balance;
  }
}
```

Outside the class, `account.pin` and `account.balance` both give errors. Writing `account.accountNumber = "999"` fails too, because a readonly value can only be set where we declare it or inside the constructor.

One last thing. `private` and `protected` are only checked by TypeScript and they disappear in the final JavaScript. For real privacy at runtime we can use `#pin`. For normal projects these four keywords are enough, and they keep a class easy to trust.
