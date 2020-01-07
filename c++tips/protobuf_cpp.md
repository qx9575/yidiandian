#使用proto3 对于自定义类型赋值遇到的问题#



1. **非数组类型-以下三个函数**
	
    	void yyy::set_allocated_xxx(xxx)
		void Person_PhoneNumber::set_allocated_number(std::string* number)




    	release_xxx()
    	mutable_xxx()
2. **Protobuf使用不当导致的程序内存上涨问题**



