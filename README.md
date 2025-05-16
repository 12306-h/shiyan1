
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract ProjectContract {
    // 状态变量
    address public owner;
    uint256 public contractValue;
    
    // 数据结构
    struct User {
        uint256 balance;
        bool isActive;
    }
    
    // 映射和数组
    mapping(address => User) public users;
    address[] public userAddresses;
    
    // 事件
    event ValueChanged(uint256 newValue);
    event UserRegistered(address user);
    
    // 构造函数
    constructor(uint256 initialValue) {
        owner = msg.sender;
        contractValue = initialValue;
    }
    
    // 函数修饰器
    modifier onlyOwner() {
        require(msg.sender == owner, "Not the owner");
        _;
    }
    
    // 核心功能函数
    function setValue(uint256 newValue) public onlyOwner {
        contractValue = newValue;
        emit ValueChanged(newValue);
    }
    
    function registerUser() public {
        require(!users[msg.sender].isActive, "User already registered");
        users[msg.sender] = User({
            balance: 0,
            isActive: true
        });
        userAddresses.push(msg.sender);
        emit UserRegistered(msg.sender);
    }
    
    // 视图函数
    function getUserCount() public view returns (uint256) {
        return userAddresses.length;
    }
    
    // 其他辅助函数...
}