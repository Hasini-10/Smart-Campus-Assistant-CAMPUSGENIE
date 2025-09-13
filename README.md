import React, { useState, useRef, useEffect } from 'react';
import { Send, Mic, Calendar, Clock, MapPin, Phone, Mail, Users, Book, Utensils, Building2, ChevronRight, Bot, User, Settings, Sparkles } from 'lucide-react';

const SmartCampusAssistant = () => {
  const [messages, setMessages] = useState([
    {
      id: 1,
      text: "Welcome to Mallareddy Vishwavidyapeeth! ✨ I'm your Smart Campus Assistant. Ask me about schedules, facilities, dining, library services, or anything else you need help with!",
      sender: 'ai',
      timestamp: new Date().toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' })
    }
  ]);
  const [inputText, setInputText] = useState('');
  const [isTyping, setIsTyping] = useState(false);
  const [showCalendar, setShowCalendar] = useState(false);
  const messagesEndRef = useRef(null);

  const scrollToBottom = () => {
    messagesEndRef.current?.scrollIntoView({ behavior: "smooth" });
  };

  useEffect(() => {
    scrollToBottom();
  }, [messages]);

  // Today's schedule data
  const todaySchedule = [
    { time: '09:00 - 10:30', subject: 'Data Structures', room: 'Room 301', type: 'lecture' },
    { time: '10:45 - 12:15', subject: 'Machine Learning', room: 'Lab 205', type: 'lab' },
    { time: '12:40 - 01:20', subject: 'Lunch Break', room: 'Food Court', type: 'break' },
    { time: '01:30 - 03:00', subject: 'Database Systems', room: 'Room 405', type: 'lecture' },
    { time: '03:15 - 04:45', subject: 'Project Work', room: 'Lab 301', type: 'project' }
  ];

  // Quick action buttons with beautiful gradients for dark theme
  const quickActions = [
    { icon: <Calendar className="w-5 h-5" />, text: 'Class Schedule', gradient: 'from-violet-600 to-purple-700' },
    { icon: <Utensils className="w-5 h-5" />, text: 'Dining Info', gradient: 'from-emerald-600 to-green-700' },
    { icon: <Book className="w-5 h-5" />, text: 'Library', gradient: 'from-blue-600 to-indigo-700' },
    { icon: <MapPin className="w-5 h-5" />, text: 'Campus Map', gradient: 'from-orange-600 to-red-600' },
    { icon: <Building2 className="w-5 h-5" />, text: 'Admin Services', gradient: 'from-pink-600 to-rose-700' },
    { icon: <Users className="w-5 h-5" />, text: 'Events', gradient: 'from-teal-600 to-cyan-700' }
  ];

  const handleSendMessage = async () => {
    if (!inputText.trim()) return;

    const userMessage = {
      id: Date.now(),
      text: inputText,
      sender: 'user',
      timestamp: new Date().toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' })
    };

    setMessages(prev => [...prev, userMessage]);
    setInputText('');
    setIsTyping(true);

    // Simulate AI response
    setTimeout(() => {
      const aiResponse = generateAIResponse(inputText);
      setMessages(prev => [...prev, {
        id: Date.now() + 1,
        text: aiResponse,
        sender: 'ai',
        timestamp: new Date().toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' })
      }]);
      setIsTyping(false);
    }, 1500);
  };

  const generateAIResponse = (input) => {
    const lowerInput = input.toLowerCase();
    
    if (lowerInput.includes('dining') || lowerInput.includes('food') || lowerInput.includes('canteen')) {
      return "🍽️ Food Court is available on campus! Please note: Food court access is only during lunch break (12:40 PM - 1:20 PM). Students are not allowed during class hours. We serve fresh, hygienic meals with variety of options including North & South Indian cuisines. 🥘✨";
    }
    
    if (lowerInput.includes('library')) {
      return "📚 The Central Library is open during all college hours (9:00 AM - 5:00 PM). We have a vast collection of books, journals, and digital resources. Note: Booking seats is strictly prohibited. It's first-come, first-served basis. Study rooms and computer lab access available! 📖💡";
    }
    
    if (lowerInput.includes('admin') || lowerInput.includes('office')) {
      return "🏢 Administrative services are available at Block A during college hours (9:00 AM - 5:00 PM). You can visit for admissions, fee payments, certificates, and other official work. Contact: +91-960-555-9999 📞";
    }
    
    if (lowerInput.includes('hostel')) {
      return "🏠 Separate hostels available for boys and girls with modern facilities including library, computers with internet, gym, and yoga facilities. All hostels are located within campus premises. Contact hostel office in Block A for more details! 🌟";
    }
    
    if (lowerInput.includes('contact') || lowerInput.includes('phone')) {
      return "📞 Contact Information:\n• Main Office: +91-960-555-9999\n• Toll Free: 1800-890-7799\n• Email: info@mrvv.edu.in\n• Address: Maisammaguda, Dulapally, Hyderabad - 500100 📍";
    }
    
    if (lowerInput.includes('event') || lowerInput.includes('fest') || lowerInput.includes('cultural')) {
      return "🎉 Upcoming Events:\n• Tech Fest 2025 - March 15-17 🚀\n• Cultural Fest - April 5-7 🎭\n• Sports Meet - February 20-22 🏆\n• Hackathon - March 1-2 💻\nStay tuned for registrations and more details! ✨";
    }

    if (lowerInput.includes('schedule') || lowerInput.includes('class') || lowerInput.includes('time')) {
      return "📅 Your today's schedule is displayed in the sidebar! College hours: 9:00 AM - 5:00 PM. Lunch break: 12:40 PM - 1:20 PM. For detailed weekly schedule, check your student portal or ask me specific questions! 🕒";
    }
    
    return "I'm here to help you with campus information! ✨ Ask me about dining, library, administrative services, hostels, events, or any other campus-related queries. How can I assist you today? 😊🎯";
  };

  const handleQuickAction = (action) => {
    setInputText(action.text);
    handleSendMessage();
  };

  const getTypeColor = (type) => {
    switch (type) {
      case 'lecture': return 'bg-gradient-to-r from-blue-900/80 to-indigo-800/80 text-blue-200 border border-blue-700/50';
      case 'lab': return 'bg-gradient-to-r from-emerald-900/80 to-green-800/80 text-emerald-200 border border-emerald-700/50';
      case 'break': return 'bg-gradient-to-r from-orange-900/80 to-amber-800/80 text-orange-200 border border-orange-700/50';
      case 'project': return 'bg-gradient-to-r from-purple-900/80 to-violet-800/80 text-purple-200 border border-purple-700/50';
      default: return 'bg-gradient-to-r from-gray-800/80 to-slate-700/80 text-gray-300 border border-gray-600/50';
    }
  };

  return (
    <div className="max-w-6xl mx-auto bg-gray-900 shadow-2xl rounded-3xl overflow-hidden border border-gray-700">
      {/* Beautiful Dark Header with Gradient */}
      <div className="bg-gradient-to-br from-slate-800 via-gray-800 to-gray-900 text-white p-6 relative overflow-hidden border-b border-gray-700">
        <div className="absolute inset-0 bg-gradient-to-r from-indigo-900/20 to-purple-900/20 backdrop-blur-sm"></div>
        <div className="relative z-10 flex items-center justify-between">
          <div className="flex items-center space-x-4">
            <div className="w-16 h-16 bg-gradient-to-br from-indigo-600 to-purple-700 rounded-2xl flex items-center justify-center shadow-xl border border-indigo-500/30">
              <Bot className="w-8 h-8 text-white" />
            </div>
            <div>
              <h1 className="text-2xl font-bold flex items-center text-white">
                Smart Campus Assistant 
                <Sparkles className="w-6 h-6 ml-2 text-yellow-400 animate-pulse" />
              </h1>
              <p className="text-gray-300 font-medium">Mallareddy Vishwavidyapeeth Technical Campus</p>
              <div className="flex items-center mt-2">
                <div className="w-2 h-2 bg-emerald-400 rounded-full mr-2 animate-pulse shadow-lg"></div>
                <span className="text-sm text-gray-300">Online & Ready to Help</span>
              </div>
            </div>
          </div>
          <div className="p-2 bg-gray-700/50 backdrop-blur-md rounded-xl border border-gray-600/50 hover:bg-gray-600/50 transition-all cursor-pointer">
            <Settings className="w-6 h-6 text-gray-300 hover:text-white transition-colors" />
          </div>
        </div>
      </div>

      <div className="flex flex-col lg:flex-row min-h-[600px]">
        {/* Main Chat Section */}
        <div className="flex-1 flex flex-col bg-gradient-to-b from-gray-900 to-gray-800">
          {/* Enhanced Quick Actions */}
          <div className="p-6 bg-gradient-to-r from-gray-800 to-slate-800 border-b border-gray-700">
            <h3 className="text-sm font-bold text-gray-200 mb-4 flex items-center">
              <Sparkles className="w-4 h-4 mr-2 text-indigo-400" />
              Quick Actions
            </h3>
            <div className="grid grid-cols-2 md:grid-cols-3 gap-3">
              {quickActions.map((action, index) => (
                <button
                  key={index}
                  onClick={() => handleQuickAction(action)}
                  className={`bg-gradient-to-r ${action.gradient} text-white p-4 rounded-2xl flex items-center space-x-3 hover:scale-105 transition-all duration-200 shadow-lg hover:shadow-xl border border-white/10 backdrop-blur-sm`}
                >
                  <div className="p-1 bg-white/20 rounded-lg backdrop-blur-sm">
                    {action.icon}
                  </div>
                  <span className="text-sm font-semibold">{action.text}</span>
                </button>
              ))}
            </div>
          </div>

          {/* Messages with Beautiful Dark Styling */}
          <div className="flex-1 p-6 space-y-4 max-h-96 overflow-y-auto bg-gradient-to-b from-gray-900 to-gray-800">
            {messages.map((message) => (
              <div key={message.id} className={`flex ${message.sender === 'user' ? 'justify-end' : 'justify-start'}`}>
                <div className={`max-w-xs lg:max-w-md px-5 py-4 rounded-2xl shadow-lg border ${
                  message.sender === 'user'
                    ? 'bg-gradient-to-r from-indigo-600 to-purple-700 text-white border-indigo-500/30'
                    : 'bg-gray-800 border-gray-600 shadow-md text-gray-100'
                }`}>
                  <div className="flex items-start space-x-3">
                    {message.sender === 'ai' && (
                      <div className="w-8 h-8 bg-gradient-to-r from-indigo-600 to-purple-700 rounded-xl flex items-center justify-center flex-shrink-0 mt-1 shadow-lg">
                        <Bot className="w-4 h-4 text-white" />
                      </div>
                    )}
                    <div className="flex-1">
                      <p className="text-sm leading-relaxed whitespace-pre-line">{message.text}</p>
                      <p className={`text-xs mt-2 ${
                        message.sender === 'user' ? 'text-indigo-200' : 'text-gray-400'
                      }`}>
                        {message.timestamp}
                      </p>
                    </div>
                    {message.sender === 'user' && (
                      <div className="w-8 h-8 bg-white/20 rounded-xl flex items-center justify-center flex-shrink-0 mt-1 backdrop-blur-sm">
                        <User className="w-4 h-4 text-white" />
                      </div>
                    )}
                  </div>
                </div>
              </div>
            ))}
            
            {isTyping && (
              <div className="flex justify-start">
                <div className="bg-gray-800 px-5 py-4 rounded-2xl shadow-lg border border-gray-600">
                  <div className="flex items-center space-x-3">
                    <div className="w-8 h-8 bg-gradient-to-r from-indigo-600 to-purple-700 rounded-xl flex items-center justify-center shadow-lg">
                      <Bot className="w-4 h-4 text-white" />
                    </div>
                    <div className="flex space-x-1">
                      <div className="w-2 h-2 bg-indigo-400 rounded-full animate-bounce"></div>
                      <div className="w-2 h-2 bg-purple-400 rounded-full animate-bounce" style={{ animationDelay: '0.1s' }}></div>
                      <div className="w-2 h-2 bg-blue-400 rounded-full animate-bounce" style={{ animationDelay: '0.2s' }}></div>
                    </div>
                  </div>
                </div>
              </div>
            )}
            <div ref={messagesEndRef} />
          </div>

          {/* Beautiful Input Section */}
          <div className="p-6 bg-gradient-to-r from-gray-800 to-slate-800 border-t border-gray-700">
            <div className="flex space-x-4">
              <div className="flex-1 relative">
                <input
                  type="text"
                  value={inputText}
                  onChange={(e) => setInputText(e.target.value)}
                  onKeyPress={(e) => e.key === 'Enter' && handleSendMessage()}
                  placeholder="Ask me anything about campus..."
                  className="w-full px-6 py-4 pr-14 rounded-2xl border border-gray-600 focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:border-transparent shadow-lg bg-gray-700 backdrop-blur-sm transition-all text-white placeholder-gray-400"
                />
                <button className="absolute right-4 top-1/2 transform -translate-y-1/2 text-gray-400 hover:text-indigo-400 transition-colors p-1 rounded-lg hover:bg-gray-600/50">
                  <Mic className="w-5 h-5" />
                </button>
              </div>
              <button
                onClick={handleSendMessage}
                className="bg-gradient-to-r from-indigo-600 to-purple-700 text-white px-8 py-4 rounded-2xl hover:from-indigo-700 hover:to-purple-800 transition-all transform hover:scale-105 shadow-lg hover:shadow-xl border border-indigo-500/30"
              >
                <Send className="w-5 h-5" />
              </button>
            </div>
          </div>
        </div>

        {/* Beautiful Dark Sidebar */}
        <div className="w-full lg:w-80 bg-gradient-to-b from-gray-800 to-gray-900 border-l border-gray-700">
          {/* Today's Schedule */}
          <div className="p-6 border-b border-gray-700">
            <div className="flex items-center justify-between mb-5">
              <h3 className="font-bold text-gray-100 flex items-center">
                <div className="p-2 bg-gradient-to-r from-indigo-600 to-purple-700 rounded-lg mr-3 shadow-lg">
                  <Calendar className="w-4 h-4 text-white" />
                </div>
                Today's Schedule
              </h3>
              <button
                onClick={() => setShowCalendar(!showCalendar)}
                className="text-indigo-400 hover:text-indigo-300 text-sm font-medium flex items-center px-3 py-1 rounded-lg hover:bg-gray-700/50 transition-all"
              >
                View Calendar <ChevronRight className="w-4 h-4 ml-1" />
              </button>
            </div>
            <div className="space-y-4">
              {todaySchedule.map((item, index) => (
                <div key={index} className="bg-gray-700/50 p-4 rounded-xl shadow-md border border-gray-600/50 hover:shadow-lg transition-all hover:scale-102 backdrop-blur-sm">
                  <div className="flex items-center justify-between mb-2">
                    <span className="text-sm font-bold text-gray-100">{item.subject}</span>
                    <span className={`px-3 py-1 rounded-xl text-xs font-semibold ${getTypeColor(item.type)}`}>
                      {item.type}
                    </span>
                  </div>
                  <div className="text-xs text-gray-400 space-y-1">
                    <div className="flex items-center">
                      <Clock className="w-3 h-3 mr-2 text-indigo-400" />
                      {item.time}
                    </div>
                    <div className="flex items-center">
                      <MapPin className="w-3 h-3 mr-2 text-purple-400" />
                      {item.room}
                    </div>
                  </div>
                </div>
              ))}
            </div>
          </div>

          {/* Campus Facilities */}
          <div className="p-6">
            <h3 className="font-bold text-gray-100 mb-5 flex items-center">
              <div className="p-2 bg-gradient-to-r from-emerald-600 to-teal-700 rounded-lg mr-3 shadow-lg">
                <Building2 className="w-4 h-4 text-white" />
              </div>
              Campus Facilities
            </h3>
            <div className="space-y-4 text-sm">
              <div className="bg-gradient-to-r from-emerald-900/50 to-green-800/50 p-4 rounded-xl shadow-md border border-emerald-700/30 backdrop-blur-sm">
                <div className="flex items-center text-emerald-300 mb-3">
                  <div className="p-1 bg-emerald-600 rounded-lg mr-2">
                    <Utensils className="w-3 h-3 text-white" />
                  </div>
                  <span className="font-bold">Food Court</span>
                </div>
                <p className="text-gray-300 text-xs mb-1">Open: 12:40 PM - 1:20 PM (Lunch only)</p>
                <p className="text-red-400 text-xs font-medium">⚠️ No access during class hours</p>
              </div>

              <div className="bg-gradient-to-r from-blue-900/50 to-indigo-800/50 p-4 rounded-xl shadow-md border border-blue-700/30 backdrop-blur-sm">
                <div className="flex items-center text-blue-300 mb-3">
                  <div className="p-1 bg-blue-600 rounded-lg mr-2">
                    <Book className="w-3 h-3 text-white" />
                  </div>
                  <span className="font-bold">Central Library</span>
                </div>
                <p className="text-gray-300 text-xs mb-1">Open: 9:00 AM - 5:00 PM</p>
                <p className="text-red-400 text-xs font-medium">⚠️ Seat booking prohibited</p>
              </div>

              <div className="bg-gradient-to-r from-purple-900/50 to-pink-800/50 p-4 rounded-xl shadow-md border border-purple-700/30 backdrop-blur-sm">
                <div className="flex items-center text-purple-300 mb-3">
                  <div className="p-1 bg-purple-600 rounded-lg mr-2">
                    <Building2 className="w-3 h-3 text-white" />
                  </div>
                  <span className="font-bold">Admin Services</span>
                </div>
                <p className="text-gray-300 text-xs mb-1">Block A: 9:00 AM - 5:00 PM</p>
                <p className="text-gray-400 text-xs">Admissions, Fees, Certificates</p>
              </div>
            </div>
          </div>
        </div>
      </div>

      {/* Stunning Dark Footer */}
      <div className="bg-gradient-to-r from-gray-900 via-slate-900 to-gray-900 text-white p-8 border-t border-gray-700">
        <div className="grid grid-cols-1 md:grid-cols-3 gap-8 text-sm">
          <div>
            <h4 className="font-bold mb-4 text-indigo-400 flex items-center">
              <Phone className="w-4 h-4 mr-2" />
              Contact Information
            </h4>
            <div className="space-y-3 text-gray-400">
              <div className="flex items-center hover:text-gray-200 transition-colors">
                <Phone className="w-4 h-4 mr-3 text-indigo-400" />
                <span>+91-960-555-9999</span>
              </div>
              <div className="flex items-center hover:text-gray-200 transition-colors">
                <Phone className="w-4 h-4 mr-3 text-emerald-400" />
                <span>1800-890-7799 (Toll Free)</span>
              </div>
              <div className="flex items-center hover:text-gray-200 transition-colors">
                <Mail className="w-4 h-4 mr-3 text-purple-400" />
                <span>info@mrvv.edu.in</span>
              </div>
            </div>
          </div>

          <div>
            <h4 className="font-bold mb-4 text-emerald-400 flex items-center">
              <MapPin className="w-4 h-4 mr-2" />
              Campus Address
            </h4>
            <div className="text-gray-400 leading-relaxed">
              <div className="flex items-start hover:text-gray-200 transition-colors">
                <MapPin className="w-4 h-4 mr-3 mt-1 flex-shrink-0 text-emerald-400" />
                <span>
                  Mallareddy Vishwavidyapeeth<br />
                  Maisammaguda, Dulapally<br />
                  Hyderabad, Telangana - 500100
                </span>
              </div>
            </div>
          </div>

          <div>
            <h4 className="font-bold mb-4 text-purple-400 flex items-center">
              <Sparkles className="w-4 h-4 mr-2" />
              Quick Links
            </h4>
            <div className="space-y-2 text-gray-400">
              <div className="hover:text-gray-200 transition-colors cursor-pointer">• Administrative Block A</div>
              <div className="hover:text-gray-200 transition-colors cursor-pointer">• UGC Approved Deemed University</div>
              <div className="hover:text-gray-200 transition-colors cursor-pointer">• Founded by Shri Malla Reddy</div>
              <div className="hover:text-gray-200 transition-colors cursor-pointer">• Technical Campus Excellence</div>
            </div>
          </div>
        </div>
        
        <div className="border-t border-gray-700 mt-8 pt-6 text-center">
          <p className="text-gray-500 text-sm">
            © 2025 Mallareddy Vishwavidyapeeth. Smart Campus Assistant - Powered by AI Technology ✨
          </p>
        </div>
      </div>
    </div>
  );
};

export default SmartCampusAssistant;
