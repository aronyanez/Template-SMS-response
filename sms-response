<?php 
/* Template Name: Respuesta SMS  */  

global $wpdb;
require(dirname(__FILE__) . '/inc/quiubas1.2.5/Quiubas.php');
\Quiubas\Quiubas::setAuth( '5f3ad87a1a68a775dffba5db4bebdd08f357e358', '43d040bdacf65bffd59305bb4fd38e18df54229e' );
$data ='{"source_addr": "524434395115","message": "Vidaid IpUUWa","sms_id": "true"}';
//1$data = json_decode(file_get_contents( 'php://input' ));
$data = json_decode($data);
$code = substr($data->message, 7);
$templateSms = "Código incorrecto";
$wristbandData = $wpdb->get_row( "SELECT data_personal,data_medical FROM wp_wristband WHERE status = 'enabled' AND key_wristband = '$code'" );

if(isset($wristbandData->data_personal)) {
	$listAllergy = '';
	$listSickness = '';
	$listInjury = '';
	$wristbandDataPersonal = json_decode($wristbandData->data_personal);
	$wristbandDataMedical = json_decode($wristbandData->data_medical);
	if(isset($wristbandDataMedical->listAllergy) && !empty($wristbandDataMedical->listAllergy)) {
		foreach ($wristbandDataMedical->listAllergy as $allergy) {
			$listAllergy.=$allergy.', ';
		}
		$listAllergy = substr($listAllergy, 0, -2);
	}
	else {
		$listAllergy = 'Sin Alergias';
	}
	if(isset($wristbandDataMedical->listSickness) && !empty($wristbandDataMedical->listSickness) ) {
		foreach ($wristbandDataMedical->listSickness as $sicknes) {
			$listSickness.=$sicknes.', ';

		}
		$listSickness.=substr($listSickness,0,-2);
	}
	else {
		$listSickness = 'Sin Enfermedades';
	}
	if(isset($wristbandDataMedical->listInjury)  && !empty($wristbandDataMedical->listInjury) ) {
		for($i=0; $i<count($wristbandDataMedical->listInjury); $i++)
			$listInjury .= $wristbandDataMedical->listInjury[$i]->lesion.', ';
		$listInjury = substr($listInjury, 0, -2);
	}
	else {
		$listInjury = 'Sin Lesiones';
	}
		//quitar coma final





	$templateSms = 
	'Nombre: '.$wristbandDataPersonal->name.
	'. Fecha de Nacimiento: '.$wristbandDataPersonal->date.
	'. Tipo de Sangre: '.$wristbandDataMedical->tipoSangre.
	'. Alergias: '.$listAllergy.
	'. Enfermedades: '.$listSickness.
	'. Lesiones de Cuidado: '.$listInjury.
	'. Donador de Organos: '.$wristbandDataMedical->donadorOrganos.
	'. NSS o Poliza: '.$wristbandDataPersonal->poliza.' '.$wristbandDataPersonal->noSeguro;
}

	$templateSmsarray= str_split($templateSms,230);
	foreach ($templateSmsarray as $key => $SMSection) {
		sendSMS($SMSection,$data->source_addr);
		sendcorreo($SMSection);
		sleep(1);
	}




function sendSMS( $template, $number ) {
	$result = \Quiubas\Sms::send(array(
		'to_number' => $number,
		'message' => $template,
	));
}


function sendcorreo($template) {
	$to = 'aron@g4a.mx';
	$subject = 'The subject';
	$body = $template;
	$headers = array('Content-Type: text/html; charset=UTF-8');

	wp_mail( $to, $subject, $body, $headers );
}



die();
