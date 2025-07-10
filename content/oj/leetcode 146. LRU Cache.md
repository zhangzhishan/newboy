---
title: leetcode 146. LRU Cache
tags: leetcode, oj
date: 25 Apr 2017 10:48 PM
---
	Design and implement a data structure for Least Recently Used (LRU) cache. It should support the following operations: get and put.
	
	get(key) - Get the value (will always be positive) of the key if the key exists in the cache, otherwise return -1.
	put(key, value) - Set or insert the value if the key is not already present. When the cache reached its capacity, it should invalidate the least recently used item before inserting a new item.
	
	Follow up:
	Could you do both operations in O(1) time complexity?


	class LRUCache {
	    // 基本思想是用一个 map 保存所有的对，用于查找，
	    // 用一个双向列表维持顺序。
	    int m_capacity;
	    unordered_map<int,  list<pair<int, int>>::iterator> m_map; //m_map_iter->first: key, m_map_iter->second: list iterator;
	    list<pair<int, int>> m_list; //m_list_iter->first: key, m_list_iter->second: value;
	public:
	    LRUCache(int capacity):m_capacity(capacity) {
	
	    }
	
	    int get(int key) {
	        auto found_iter = m_map.find(key);
	        if (found_iter == m_map.end()) //key doesn't exist
	            return -1;
	        m_list.splice(m_list.begin(), m_list, found_iter->second); //move the node corresponding to key to front
	        return found_iter->second->second;   
	    }
	
	    void put(int key, int value) {
	        auto found_iter = m_map.find(key);
	        if (found_iter != m_map.end()) //key exists
	        {
	            m_list.splice(m_list.begin(), m_list, found_iter->second); //move the node corresponding to key to front
	            found_iter->second->second = value;                        //update value of the node
	            return;
	        }
	        if (m_map.size() == m_capacity) //reached capacity
	        {
	           int key_to_del = m_list.back().first; 
	           m_list.pop_back();            //remove node in list;
	           m_map.erase(key_to_del);      //remove key in map
	        }
	        m_list.emplace_front(key, value);  //create new node in list
	        m_map[key] = m_list.begin();       //create correspondence between key and node
	    }
	};
	
	/**
	 * Your LRUCache object will be instantiated and called as such:
	 * LRUCache obj = new LRUCache(capacity);
	 * int param_1 = obj.get(key);
	 * obj.put(key,value);
	 */