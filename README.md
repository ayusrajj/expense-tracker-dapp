# On-Chain Expense Tracker dApp – Feature Enhancements

This project includes two new core features added to the decentralized expense tracker dApp:

---

## ✅ Features Added

### 1. 📊 Display Number of Registered Users

Displays the total number of users registered on the blockchain via the smart contract.

#### Frontend (React)

**State and Fetch Logic:**
```js
const [totalUsers, setTotalUsers] = useState(0);

useEffect(() => {
  const fetchTotalUsers = async () => {
    if (contract) {
      const users = await contract.methods.getTotalUsers().call();
      setTotalUsers(users);
    }
  };
  fetchTotalUsers();
}, [contract]);
```
#### UI(CSS)
```jsx
<p className="text-sm text-gray-300">Total Users: {totalUsers}</p>
```
#### Solidity
```sol
mapping(address => bool) public isUserRegistered;
address[] public userAddresses;

function registerUser() public {
    require(!isUserRegistered[msg.sender], "Already registered");
    isUserRegistered[msg.sender] = true;
    userAddresses.push(msg.sender);
}

function getTotalUsers() public view returns (uint) {
    return userAddresses.length;
}
```

### 2.Update User Name
Allows users to update their on-chain profile name.
#### Frontend (React)
```js
const [newName, setNewName] = useState("");

const handleUpdateName = async () => {
  if (contract && currentAccount) {
    await contract.methods.updateMyName(newName).send({ from: currentAccount });
    alert("Name updated!");
  }
};
```
#### UI(CSS)
```jsx
<input
  type="text"
  value={newName}
  onChange={(e) => setNewName(e.target.value)}
  placeholder="Enter new name"
  className="input"
/>
<button onClick={handleUpdateName} className="btn">Update Name</button>
```
#### Solidity
```sol
mapping(address => string) public userNames;

function updateMyName(string memory newName) public {
    require(isUserRegistered[msg.sender], "User not registered");
    userNames[msg.sender] = newName;
}

function getMyName() public view returns (string memory) {
    return userNames[msg.sender];
}
```
### 3.Profile Section Feature
#### Frontend (React + Tailwind CSS)
```jsx
import React, { useState } from "react";
import { ConnectButton } from "@rainbow-me/rainbowkit";

const ProfileSection = ({ currentAccount, onLogout }) => {
  const [isLoggedIn, setIsLoggedIn] = useState(!!currentAccount);

  const handleLogin = () => {
    setIsLoggedIn(true);
  };

  const handleLogout = () => {
    setIsLoggedIn(false);
    onLogout();
  };

  return (
    <div className="fixed top-4 left-4 bg-white bg-opacity-90 p-4 rounded-2xl shadow-xl w-60">
      <div className="flex items-center space-x-4">
        <img
          src="/profile.jpg"
          alt="User"
          className="w-12 h-12 rounded-full border border-gray-300"
        />
        <div>
          <p className="text-xs text-gray-600">Wallet:</p>
          <p className="text-sm font-bold text-gray-900 break-words">
            {currentAccount
              ? `${currentAccount.substring(0, 6)}...${currentAccount.substring(currentAccount.length - 4)}`
              : "Not Connected"}
          </p>
        </div>
      </div>

      <div className="mt-4 space-y-2">
        {!isLoggedIn ? (
          <button
            onClick={handleLogin}
            className="w-full py-1 text-sm bg-green-500 text-white rounded-lg"
          >
            Login
          </button>
        ) : (
          <button
            onClick={handleLogout}
            className="w-full py-1 text-sm bg-red-500 text-white rounded-lg"
          >
            Logout
          </button>
        )}
      </div>

      <div className="mt-4">
        <ConnectButton />
      </div>
    </div>
  );
};

export default ProfileSection;Frontend (React + Tailwind CSS)
```
## 📸 Screenshot

![App Screenshot](./screenshot.png)
## Made by Ayush Raj
