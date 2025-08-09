import React, { useState } from 'react';
import {
  View,
  Text,
  StyleSheet,
  ScrollView,
  TouchableOpacity,
  Switch,
  Platform,
  StatusBar,
  Alert,
} from 'react-native';
import { LinearGradient } from 'expo-linear-gradient';
import { Bell, Shield, Heart, MapPin, Users, CreditCard, HelpCircle, LogOut, ChevronRight, Globe, Moon, Volume, Eye, Lock, Smartphone, Star } from 'lucide-react-native';
import * as Haptics from 'expo-haptics';

export default function SettingsScreen() {
  const [notifications, setNotifications] = useState(true);
  const [location, setLocation] = useState(true);
  const [darkMode, setDarkMode] = useState(false);
  const [sounds, setSounds] = useState(true);
  const [showOnline, setShowOnline] = useState(true);
  const [readReceipts, setReadReceipts] = useState(true);

  const triggerHaptic = () => {
    if (Platform.OS !== 'web') {
      Haptics.impactAsync(Haptics.ImpactFeedbackStyle.Light);
    }
  };

  const settings: SettingItem[] = [
    {
      id: 'notifications',
      title: 'الإشعارات',
      subtitle: 'تفعيل أو إيقاف الإشعارات',
      icon: <Bell color="#fff" size={20} />,
      type: 'toggle',
      value: notifications,
      onPress: () => setNotifications(!notifications),
    },
    {
      id: 'location',
      title: 'مشاركة الموقع',
      subtitle: 'تفعيل الموقع للبحث عن أشخاص قريبين',
      icon: <MapPin color="#fff" size={20} />,
      type: 'toggle',
      value: location,
      onPress: () => setLocation(!location),
    },
    {
      id: 'darkMode',
      title: 'الوضع الليلي',
      subtitle: 'تغيير المظهر إلى داكن',
      icon: <Moon color="#fff" size={20} />,
      type: 'toggle',
      value: darkMode,
      onPress: () => setDarkMode(!darkMode),
    },
    {
      id: 'premium',
      title: 'الاشتراك المميز',
      subtitle: 'احصل على مزايا إضافية',
      icon: <Star color="#fff" size={20} />,
      type: 'navigation',
      onPress: () => Alert.alert('الاشتراك المميز', 'هنا يمكنك إضافة صفحة الاشتراك'),
    },
    {
      id: 'logout',
      title: 'تسجيل الخروج',
      icon: <LogOut color="#fff" size={20} />,
      type: 'action',
      destructive: true,
      onPress: () => Alert.alert('تسجيل الخروج', 'تم تسجيل خروجك بنجاح'),
    },
  ];

  const renderItem = (item: SettingItem) => (
    <TouchableOpacity
      key={item.id}
      style={[
        styles.item,
        item.destructive ? { backgroundColor: '#ff4d4d' } : {}
      ]}
      onPress={() => {
        triggerHaptic();
        if (item.type !== 'toggle') {
          item.onPress && item.onPress();
        }
      }}
      activeOpacity={0.8}
    >
      <View style={styles.icon}>{item.icon}</View>
      <View style={styles.textContainer}>
        <Text style={styles.title}>{item.title}</Text>
        {item.subtitle && <Text style={styles.subtitle}>{item.subtitle}</Text>}
      </View>
      {item.type === 'toggle' ? (
        <Switch
          value={item.value}
          onValueChange={item.onPress}
          trackColor={{ true: '#ff6699', false: '#ccc' }}
        />
      ) : (
        <ChevronRight color="#fff" />
      )}
    </TouchableOpacity>
  );

  return (
    <LinearGradient colors={['#ff6699', '#ff3366']} style={styles.container}>
      <StatusBar barStyle="light-content" />
      <ScrollView contentContainerStyle={styles.scrollContainer}>
        {settings.map(renderItem)}
      </ScrollView>
    </LinearGradient>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
  },
  scrollContainer: {
    padding: 16,
  },
  item: {
    flexDirection: 'row',
    alignItems: 'center',
    backgroundColor: 'rgba(255,255,255,0.1)',
    borderRadius: 12,
    padding: 16,
    marginBottom: 12,
  },
  icon: {
    marginRight: 12,
  },
  textContainer: {
    flex: 1,
  },
  title: {
    color: '#fff',
    fontSize: 16,
    fontWeight: 'bold',
  },
  subtitle: {
    color: '#ddd',
    fontSize: 12,
  },
});

interface SettingItem {
  id: string;
  title: string;
  subtitle?: string;
  icon: React.ReactNode;
  type: 'toggle' | 'navigation' | 'action';
  value?: boolean;
  onPress?: () => void;
  destructive?: boolean;
}
