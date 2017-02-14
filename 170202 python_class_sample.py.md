



class Shop:


class Shop:


Last login: Thu Feb  2 14:46:03 on ttys002
➜  ~ cd-py
(kizmo-py) ➜  python git:(master) ✗ cd class
(class-py) ➜  class git:(master) ✗ ls

~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
~
"class_sample.py" 17L, 374C written
class Shop:
    description = 'Python shop Class'

    def __init__(self, name, shop_type, address):
        self.__name = name
        self.__shop_type = shop_type
        self.__address = address


    def show_info(self):
        print('상점({})\n 유형:{}\n 주소:{}'.format(self._name, self._shop_type, self._address))

    def change_type(self, shop_type):
        self.__shop_type = shop_type

    @classmethod
    def change_desc(cls, description):
        cls.description = description

    @staticmethod
    def print_test():
        print('staticmethod test!')

    def get_name(self):
        return self.name__name

    def set_name(self, new_name):
        self.__name = new_name

    def get_name(self):
        return self.name__name

    def set_name(self, new_name):
        self.__name = new_name



    @property
    def name(self):
        self.__name.replace()
        return self.__name[:1] + '**'

    @name.setter
    def name(self, new_name):
        if '맥도날드' in new_name:
            print('그이름은 사용할 수 없습니다')
            return
        self.__name = new_name
        print('sef new name ({})'.format(self.__name))
-- INSERT --